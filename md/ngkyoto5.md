AnglarのAnimationあれこれ
2017.03.20 @ng-kyoto meetup
---
## Who?
### Name: Masui Masanori

### Twitter: [@masanori_msl](https://twitter.com/masanori_msl)
<img src="https://pbs.twimg.com/profile_images/832894991387152384/RNwtLhN5.jpg" alt="twitter" style="width: 128px; height: 128px; position: absolute; top:20%; right: 19%;"></img>

### Blog: [vaguely](http://mslgt.hatenablog.com/)

### GitHub: https://github.com/masanori840816
---
## Agenda
### Angularを使ってGUIをアニメーションさせてみる
---
## Sample
* [NgGuiAnimeSample - GitHub](https://github.com/masanori840816/NgGuiAnimeSample)
---
## Sample1 ボタンの拡大・縮小
### やりたいこと
* ボタンが押されたら拡大する
* もう一度押したら元に戻す
---
## Sample1 Demo
---
## Sample1 Code1
### gui-anime-sample-1.component.ts
```xml 
import { Component,Input,trigger,state,style,
  transition,animate,OnInit } from '@angular/core';
@Component({
  selector: 'app-gui-anime-sample-1',
  templateUrl: './gui-anime-sample-1.component.html',
  styleUrls: ['./gui-anime-sample-1.component.css'],
  animations: [
    trigger('buttonState', [
      state('inactive', style({
        backgroundColor: '#eee',
        transform: 'scale(1)'
      })),
      state('active',   style({
        backgroundColor: '#cfd8dc',
        transform: 'scale(1.5)'
      })),
      transition('inactive => active', animate('0ms 200ms ease-in')),
      transition('active => inactive', animate('0ms 200ms ease-out'))
    ])
  ]
})
```
---
## Sample1 Code1-1
### Animations
#### お話の中心はこの辺り
``` xml
animations: [
    trigger('buttonState', [
      state('inactive', style({
        backgroundColor: '#eee',
        transform: 'scale(1)'
      })),
      state('active',   style({
        backgroundColor: '#cfd8dc',
        transform: 'scale(1.5)'
      })),
      transition('inactive => active', animate('0ms 200ms ease-in')),
      transition('active => inactive', animate('0ms 200ms ease-out'))
    ])
  ]
```
---
## Sample1 Code1-2
### Trigger
トリガーとして指定された"buttonState"の値が変わるとアニメーション実行
``` xml
trigger('buttonState', [
```
---
## Sample1 Code1-3
### State
ここでは2つの状態を設定して、<br>
アニメーションでそれらをなめらかにつなぐ

``` xml
state('inactive', style({
    backgroundColor: '#eee',
    transform: 'scale(1)'
})),
state('active',   style({
    backgroundColor: '#cfd8dc',
    transform: 'scale(1.5)'
})),
```
---
## Sample1 Code1-4
### Transition
アニメーションで切り替える状態と、<br>
どんな風にアニメーションするかを指定する。
``` xml
transition('inactive => active', animate('0ms 200ms ease-in')),
transition('active => inactive', animate('0ms 200ms ease-out'))
```
---
## Void & Wildcard
### Void

### * (Wildcard)
任意の状態として扱うことができる。

複数種類の状態からひとつの状態へと切り替える場合、<br>
状態に関係なく処理したい場合などに有用。

#### 例
``` xml
transition('inactive => *', animate('0ms 200ms ease-in')),
```
---
## Sample1 Code1-5
### Animate
どんな風にアニメーションするかを指定する。

指定できる値は下記の3つ
1. アニメーション開始までの待機時間
2. アニメーションする時間
3. Easing

``` xml
animate('0ms 200ms ease-in')
```
---
## Easing
速度を一定ではなく緩急をつけることで、<br>
より自然なアニメーションを実現する。

### Easingの例
* [Easing Function 早見表](http://easings.net/ja)
---
## Easing 2
※ただしCSSでは全てを選択できるわけでなく、<br>
下記から選択するかまたはベジェ曲線で指定する必要がある。

* ease （デフォルト）
* linear （はじめから終わりまで一定）
* ease-in （最初だけゆっくり）
* ease-out （最後だけゆっくり）
* ease-in-out （最初と最後だけゆっくり）
* cubic-bezier() （カスタム）

---
## Sample1 Code1-5
### トリガーを引く
Buttonにトリガーとなる"buttonState"を関連付け、<br>
クリックでその値を変更することでアニメーションを実行する。

#### gui-anime-sample-1.component.html
``` xml
<button id='sample1Button' [@buttonState]='state' (click)='onButtonClicked()'>Sample1</button>
```

#### gui-anime-sample-1.component.ts
```xml
export class GuiAnimeSample1Component implements OnInit {
  private state: string;
  constructor() { }

  ngOnInit() {
    this.state = 'inactive';
  }
  private onButtonClicked(){
    this.state = (this.state === 'active')? 'inactive': 'active'; 
  }
}
```
---
## Sample1 Code1-3
### 
