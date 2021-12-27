# Humanoid Python Program
## Pythonでヒューマノイドロボットのプログラム
このプログラムは，ヒューマノイドロボット（梶田著）のMatlabプログラムをJupyterNotebookで実行する練習をしたものです．
オリジナルのMatlabプログラムは下記のリンクにあります．

<a href ="https://github.com/s-kajita/IntroductionToHumanoidRobotics">https://github.com/s-kajita/IntroductionToHumanoidRobotics</a>

ブラウザ上のJupyterNotebookで実行するときは
「%matplotlib notebook」
を記載しておくと表示したグラフの視点をマウスのドラッグで変更できるようになります．

2021/12/26日現在：VSCodeのjupyternotebookの拡張で上記コマンドが実行されると図が表示されなくなるので，VSCodeの再起動が必要になってしまいます．


## データ構造
```python:uLINK class
class uLINK:
  def __init__(self, name, sister, child, m, mother, b ,a ,q,R,p, dq, ddq, c, I, Ir, gr, u, dw):
    self.name = name     #リンク名
    self.sister = sister #リンクの兄弟
    self.child = child   #リンクの子
    self.mother = mother #リンクの親
    self.m = m           #リンクの質量
    self.b = b           #リンクの長さ
    self.a = a           #関節の軸の方向
    self.q = q           #関節の回転角度
    self.R = R           #リンクの回転行列（絶対座標基準）
    self.p = p           #リンクの関節の位置（絶対座標基準）
    self.dq =dq          #関節の角速度
    self.ddq = ddq       #関節の角加速度
    self.c  = c          #リンクの重心位置
    self.I  = I          #リンクの慣性モーメント
    self.Ir = Ir         #リンクの
    self.gr = gr         #リンクの
    self.u  = u          #関節の入力トルク
    self.dw = dw         #リンクの角加速度
```

## 順運動学

## 逆運動学


## 動力学

