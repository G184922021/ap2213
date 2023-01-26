# タイトル

応用プログラミングレポート @G184922021 山根直弥

## 作品の概要
下の図のようなゲームセンターなどでよく見るエアーホッケーのようなものを作りました.

一人で上のパドルと下のパドルを操作し、ボールを上と下のデッドゾーンに当てないようになるべく多くの回数を往復させます.
パドルはマウスカーソルで動かし、下のパドルはマウスカーソルの動きそのままで動きますが、上のパドルはマウスカーソルと逆の方向に連動して動くようになっています.
上のパドルでボールを跳ね返すごとにスコアが加算されていき、連続でパドルにボールを当て続けると獲得できるスコアが多くなります. また、上のパドルに当てると少しずつボールのスピードが速くなっていきます.
ライフは3つあって3個目のライフがなくなるとゲームオーバーとなってしまいます.
ゲームオーバーになるとゲームオーバーカウントが溜まり、スコアとボールスピードとライフがリセットされます.
![動作画面](kadai.png)
## 代表的な変数の説明

* `paddle` :パドルは中央部分と左右の端の部分で3つに分かれていたためそれを合わせて一つのオブジェクトにした.

* `ball` :ボールの形には`SphereGeometry`を用いて、作成した.

* `Frame` :外枠を作成する際には上、下、右、左で別々に作成した.
別々に作成することで、上と下の外枠ではボールが接触した時にボールを止めるなどの作業をしやすくなる.右と左の外枠ではボールを反射させるために用いていた.

## 工夫(苦心)したところ

* 上のパドルにボールが当たった時にスピードを少しずつ上げていくプログラムを組んだ.
```javascript
if(Math.abs(ball.position.z - paddle2.position.z) < paddleR + ballR &&
       Math.abs(ball.position.x - paddle2.position.x) < (paddleL / 2) + ballR){
        //中央部分と衝突
        if(ball.position.z > paddle2.position.z){
          vz = Math.abs(vz);
          speed = speed + 0.2;
          count = count + 1;
          score = score + 100*(count/2);
        }
        //右側部分と衝突
        if(ball.position.x < paddle2.position.x + (paddleL / 2)){
          vx = Math.abs(vx);
          speed = speed + 0.2;
          count = count + 1;
          score = score + 100*(count/2);
        }
        //左側部分と衝突
        else if(ball.position.x > paddle2.position.x - (paddleL / 2)){
          vx = -Math.abs(vx);
          speed = speed + 0.2;
          count = count + 1;
          score = score + 100*(count/2);
        }
```

*　スコアの加算とライフの表示、gameoverの表示をまとめた.
```javascript
    document.getElementById("score").innerText
    = String(Math.round(score)).padStart(8, "0");
    score = score + 100*(count/2);
    document.getElementById("life").innerText
    = "○○○".substring(0, life);
    document.getElementById("gameover").innerText
    = String(Math.round(gameover)).padStart(1, "0");
```

* パドルを上下であべこべに動作するようにintersects.xを逆にして動作をさかさまにした.
```javascript
    if(intersects.x < -offset){
        intersects.x = -offset;
      }
      else if(intersects.x > offset){
        intersects.x = offset;
      }
      paddle.position.x = intersects.x;
      paddle2.position.x = -intersects.x;
```



## 感想
課題作成にあたって、アイデアを絞り出す際に一番時間をかけた.
色々やりたいことはあったが、自分のプログラミングの技量と相談しながら作成したため、自分の満足度は低い形で終わってしまった.
しかし学べたことも多かったので、次につなげていけるようにしたいと思った.