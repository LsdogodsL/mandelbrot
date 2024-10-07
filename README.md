# mandelbrot creater on Google colaboratory
%matplotlib notebook
import numpy as np
import matplotlib.pyplot as plt
import itertools

# アニメーション用
from matplotlib import animation, rc
from matplotlib.animation import PillowWriter
from IPython.display import HTML

def judge_mandelbrot(c, N):
  z = 0
  for _ in range(N):
      z = z*z + c
  return np.abs(z) < 1 # N回後に絶対値が1より小さいときは発散しないと判定

N = 50 # 判定時のループ回数の設定
res = 1000 # 解像度

reals = np.linspace(-2, 0.6, num=res) # 実軸のメッシュ
imags = np.linspace(-1.1, 1.1, num=res) # 虚軸のメッシュ

mandelbrots = []
for x, y in itertools.product(reals, imags): # 格子点上の(x, y)に対してループ
    c = complex(x, y) # 複素数の候補 # Use the built-in complex function instead of np.complex
    if judge_mandelbrot(c, N): # 判定
        mandelbrots.append([c.real, c.imag]) # 判定がTrueならマンデルブロ集合に加える

mandelbrots = np.array(mandelbrots) # 型変換
print(mandelbrots.shape) # 結果の数を確認
marker_size = 0.5

fig, ax = plt.subplots(figsize=(5,5), facecolor='k')
ax.scatter(mandelbrots[:,0], mandelbrots[:,1], s=marker_size, color = 'w')
ax.set_facecolor('k')
ax.set_aspect('equal')
ax.get_xaxis().set_visible(False)
ax.get_yaxis().set_visible(False)
d = 20 # 小さい値ほど高負荷

fig, ax = plt.subplots()
def update(i):
    if d*i + d > len(mandelbrots):
        z = mandelbrots[d*i:]
    else:
        z = mandelbrots[d*i: d*i + d]
    ax.scatter(z[:, 0], z[:, 1], s=marker_size)
    ax.set_facecolor('k')
    ax.set_aspect('equal')
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)
    ax.set_xlim([-2, 0.6])
    ax.set_ylim([-1.1, 1.1])

anim = animation.FuncAnimation(fig, update, frames=len(mandelbrots)//d + 1, interval=20)
rc('animation', html='jshtml')
anim
