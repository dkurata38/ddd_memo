# ニューラルネットワーク
## パラメータの更新方法
### ADAM
一つ一つのパラメータを更新するのではなく, 一気に複数のパラメータを更新する.


### 活性化関数
#### シグモイド関数

#### ReLU関数
入力値が0より小さいときは0を戻し, 0より大きい数は入力値をそのまま戻す.
```
def relu(x):
  return np.maximum(0, x)
```

なお微分すると以下のようになる.
```
class ReLu:
  def forward(self, x):
    self.x = x
    return np.maximum(0, x)

  def backward(self, dx):
    return dx[self.x <= 0] = 0
```

#### ソフトマックス関数

## マルチタスク学習