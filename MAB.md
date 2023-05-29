# using model fit
```python
import numpy as np
import matplotlib.pyplot as plt
import random

# Setting the parameters
N = 10000
d = 9

# Building the environment inside a simulation
conversion_rates = [0.05,0.13,0.09,0.16,0.11,0.04,0.20,0.08,0.01]
X = np.array(np.zeros([N,d]))
for i in range(N):
    for j in range(d):
        if np.random.rand() <= conversion_rates[j]:
            X[i,j] = 1

# Implementing Random Selection and Thompson Sampling
strategies_selected_rs = []
strategies_selected_ts = []
total_reward_rs = 0
total_reward_ts = 0
numbers_of_rewards_1 = [0] * d
numbers_of_rewards_0 = [0] * d
for n in range(0, N):
    # Random Selection
    strategy_rs = random.randrange(d)
    strategies_selected_rs.append(strategy_rs)
    reward_rs = X[n, strategy_rs]
    total_reward_rs = total_reward_rs + reward_rs
    # Thompson Sampling
    strategy_ts = 0
    max_random = 0
    for i in range(0, d):
        random_beta = random.betavariate(numbers_of_rewards_1[i] + 1, numbers_of_rewards_0[i] + 1)
        if random_beta > max_random:
            max_random = random_beta
            strategy_ts = i
    reward_ts = X[n, strategy_ts]
    if reward_ts == 1:
        numbers_of_rewards_1[strategy_ts] = numbers_of_rewards_1[strategy_ts] + 1
    else:
        numbers_of_rewards_0[strategy_ts] = numbers_of_rewards_0[strategy_ts] + 1
    strategies_selected_ts.append(strategy_ts)
    total_reward_ts = total_reward_ts + reward_ts

# Computing the Relative Return
relative_return = (total_reward_ts - total_reward_rs) / total_reward_rs * 100
print("Relative Return: {:.0f} %".format(relative_return))
print(total_reward_rs)
print(total_reward_ts)
# Plotting the Histogram of Selections
plt.hist(strategies_selected_ts)
plt.title('Histogram of Selections')
plt.xlabel('Strategy')
plt.ylabel('Number of times the strategy was selected')
plt.show()
```

# using define model

```python

import random

import gymnasium as gym
import numpy as np

import tensorflow as tf

from keras import Model
from keras.layers import Dense


class DQN(Model):
    def __init__(self, state_size, action_size):
        super(DQN, self).__init__()

        self.replay_memory = []

        self.dense1 = Dense(48, activation="tanh", input_dim=state_size)
        self.dense2 = Dense(action_size, activation="softmax")

    def call(self, x):
        x = self.dense1(x)
        return self.dense2(x)
    
    
    def remember(self, state, action, reward, next_state, done):
        self.replay_memory.append((state, action, reward, next_state, done))
        
        
def update_model(model: DQN):
    # 리플레이 버퍼 크기가 작으면 업데이트하지 않음
    if len(model.replay_memory) < 1000:
        return
    
    # 너무 많으면 리플레이 버퍼 pop
    if len(model.replay_memory) > 20000:
        model.replay_memory.pop(0)

    # 메모리에서 랜덤 샘플링
    samples = random.sample(model.replay_memory, 64)

    # 분할
    states, actions, rewards, next_states, dones = zip(*samples)
    # states, actions, rewards, next_states, dones = zip(*model.replay_memory)

    # numpy 배열로 변환
    states = np.array(states)
    actions = np.array(actions)
    rewards = np.array(rewards)
    next_states = np.array(next_states)
    dones = np.array(dones)

    # 모델 예측
    q_values = model.call(states).numpy()
    next_q_values = model.call(next_states).numpy()

    # 타겟 값 계산
    targets = q_values.copy()
    targets[np.arange(len(rewards)), actions] = rewards + 0.99 * np.max(next_q_values, axis=1) * (1 - dones)

    # 모델 업데이트
    with tf.GradientTape() as tape:
        q_values = model.call(states)
        loss = tf.keras.losses.mean_squared_error(targets, q_values)

    gradients = tape.gradient(loss, model.trainable_variables)
    optimizer = tf.keras.optimizers.Adam(learning_rate=0.001)
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))
    
    
env = gym.make("MountainCar-v0")
# env = gym.make("MountainCar-v0", render_mode="human")
model = DQN(env.observation_space.shape[0], env.action_space.n)

for episode in range(50):
    state, info = env.reset()
    terminated = False
    truncated = False
    step = 0
    
    max_score = state[0]
    
    while not terminated and step < 1000:

        # 모델로 행동 예측
        q_values = model.call(np.array([state])).numpy()[0]
        
        # 소프트맥스 확률로 행동 선택
        action = np.random.choice(env.action_space.n, p=q_values)

        # 행동 실행
        next_state, reward, terminated, truncated, info = env.step(action)
        max_score = max(max_score, next_state[0])

        if next_state[1] > 0 and action == 2:
            reward = 5
        elif next_state[1] < 0 and action == 0:
            reward = 5
        if terminated:
            reward = 100

        # 리플레이 버퍼에 기억
        model.remember(state, action, reward, next_state, terminated)
        
        # 모델 업데이트
        update_model(model)

        state = next_state
        step += 1
    
    print("Episode: {}, Steps: {}, Max Score: {}".format(episode, step, max_score))

env.close()
model.save_weights('mountaincar2', save_format='tf')
```
