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

数式の表示には<a href ="https://tex-image-link-generator.herokuapp.com/">tex image link generator</a>を使わせていただきました．

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
    self.w  = w          #リンクの角速度
    self.vo = vo         #リンクの並進速度
    self.dw = dw         #リンクの角加速度
    self.dvo = dvo       #リンクの並進加速度
```
上記のデータ構造を利用した簡単な関数の例



### FindMther関数
上記のデータ構造のクラスで兄弟と子供のリンク構造を定義しておけば，この関数を使うことで，リンクの親の情報を求めることができる．最初にリンク構造を初期化するときに親リンクの情報も入力できるが，大変なのでこの関数を使ってデータ構造に覚えさせる手順を使うと良い．



## 順運動学

再帰関数を使った効率の良い順運動学計算



## 逆運動学

ヤコビ行列を使った逆運動学の計算とLM方を使ったロバストな逆運動学の計算を行う．<br>
<img src="https://github.com/ttakubo/Humanoid/blob/main/anim.gif">

## 動力学

４種類のシミュレーションを順に行っていく．
シミュレーションの動画は麺をつけるのが難しかったので，一筆書きになっています．．．

### ３軸の回転シミュレーション：無重力

<img src="https://github.com/ttakubo/Humanoid/blob/main/rotate.gif">

### 並進と１軸の回転のシミュレーション：無重力

<img src="https://github.com/ttakubo/Humanoid/blob/main/screwmotion.gif">

### 並進と２軸の回転のシミュレーション：無重力

<img src="https://github.com/ttakubo/Humanoid/blob/main/rigidbody_fly.gif">

### コマのシミュレーション：重力＋接触（外力）あり

<img src="https://github.com/ttakubo/Humanoid/blob/main/top_anim.gif">


### 剛体リンクの順動力学シミュレーション：床反力は適当

Humanoid Robotの本家のMatlabプログラムには，床反力の項目がないために無重力で落ちていくだけとなっていたが，InverseDynamicsの関数に重心に加わる外力項を加えることで簡易的に床反力を再現しました．

本にある親リンクと子リンクから加わる力の等式は下記の通り．<br>
<img src=
"https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Bbmatrix%7D%0Af_j++%5C%5C+%5Ctau_j+%0A%5Cend%7Bbmatrix%7D%0A%3D+I%5ES_j+%5Cdot%7B%5Cxi_j%7D+%2B+%5Cxi_j+%5Ctimes+I%5ES_j+%5Cxi_j+-%0A%5Cbegin%7Bbmatrix%7D%0Af%5EE_j+%5C%5C+%5Ctau%5EE_j+%0A%5Cend%7Bbmatrix%7D%0A%2B%0A%5Cbegin%7Bbmatrix%7D%0Af_%7Bj%2B1%7D++%5C%5C+%5Ctau_%7Bj+%2B1%7D%0A%5Cend%7Bbmatrix%7D" 
alt="\begin{bmatrix}
f_j  \\ \tau_j 
\end{bmatrix}
= I^S_j \dot{\xi_j} + \xi_j \times I^S_j \xi_j -
\begin{bmatrix}
f^E_j \\ \tau^E_j 
\end{bmatrix}
+
\begin{bmatrix}
f_{j+1}  \\ \tau_{j +1}
\end{bmatrix}">
<br>
Matlabの本家のプログラムだとリンクの重心に加わる力<br>
<img src=
"https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Bbmatrix%7D%0Af%5EE_j+%5C%5C+%5Ctau%5EE_j+%0A%5Cend%7Bbmatrix%7D" 
alt="\begin{bmatrix}
f^E_j \\ \tau^E_j 
\end{bmatrix}">
<br>
が実装されていないので，この外力項を追加すると接触や各リンクの重力を考えたシミュレーションができる．
