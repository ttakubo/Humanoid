# Humanoid Python Program
## Pythonでヒューマノイドロボットのプログラム
このプログラムは，ヒューマノイドロボット（梶田著）のMatlabプログラムをJupyterNotebookで実行する練習をしたものです．
オリジナルのMatlabプログラムは下記のリンクにあります．

<a href ="https://github.com/s-kajita/IntroductionToHumanoidRobotics">https://github.com/s-kajita/IntroductionToHumanoidRobotics</a>

ブラウザ上のJupyterNotebookで実行するときは
「%matplotlib notebook」
を記載しておくと表示したグラフの視点をマウスのドラッグで変更できるようになります．

2021/12/26日現在：VSCodeのjupyternotebookの拡張で上記コマンドが実行されると図が表示されなくなるので，VSCodeの再起動が必要になってしまいます．

定義どおりに各リンクのパラメータとリンク同士のつながりを定義するだけで，ロボット工学に必要な運動学，逆運動学，動力学，逆動力学のすべての機能を利用することができます．

プログラムの一つ一つはコードが大変短く，本に記載してある内容と一つ一つわかりやすい形で実装してあり，大変理解しやすいプログラムになっていると思います．（私のPythonプログラムの移植でわかりにくくなっているかもしれませんが．．．）

プログラムが短くなっている理由は，アルゴリズムとして再帰関数を利用しているためです．プログラムを理解する上で，<a href ="https://qiita.com/drken/items/23a4f604fa3f505dd5ad">ここ（再帰関数を学ぶと、どんな世界が広がるか）</a>の解説などを見て理解すると良いと思います．


## データ構造
動力学を含む最終的なリンクのクラスは下記の通り．
```python:uLINK class
class uLINK:
  def __init__(self, name, sister, child, m, mother, b ,a ,q,R,p, dq, ddq, c, I, Ir, gr, u, dw):
    self.name = name     #リンク名
    self.sister = sister #リンクの兄弟
    self.child = child   #リンクの子
    self.mother = mother #リンクの親
    self.m = m           #リンクの質量
    self.b = b           #リンクの長さ（親リンク相対）
    self.a = a           #関節の軸の方向(親リンク相対)
    self.q = q           #関節の回転角度
    self.R = R           #リンクの回転行列（絶対座標基準）
    self.p = p           #リンクの関節の位置（絶対座標基準）
    self.dq =dq          #関節の角速度
    self.ddq = ddq       #関節の角加速度
    self.c  = c          #リンクの重心位置
    self.I  = I          #リンクの慣性モーメント
    self.u  = u          #関節の入力トルク
    self.dw = dw         #リンクの角加速度
```
上記のデータ構造を利用した簡単な関数の例

FindMther関数


## 順運動学

再帰関数を使った効率の良い順運動学計算

<img src="https://github.com/ttakubo/Humanoid/blob/main/anim.gif">


## 逆運動学

ヤコビ行列を使った逆運動学の計算とLM方を使ったロバストな逆運動学の計算を行う．

## 動力学

４種類のシミュレーションを順に行っていく．

### ３軸の回転シミュレーション：無重力

<img src="https://github.com/ttakubo/Humanoid/blob/main/rotate.gif">

### 並進と１軸の回転のシミュレーション：無重力

<img src="https://github.com/ttakubo/Humanoid/blob/main/screwmotion.gif>

### 並進と２軸の回転のシミュレーション：無重力

<img src="https://github.com/ttakubo/Humanoid/blob/main/rigidbody_fly.gif">

### コマのシミュレーション：重力＋接触（外力）あり

<img src="https://github.com/ttakubo/Humanoid/blob/main/top_anim.gif">
