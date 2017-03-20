<div style='position:absolute; left:25%; top:35%;'>
    <h1>Angularで<br>GUI Animation</h1>
</div>
<div style='position:absolute; right: 10%; bottom:10%;'>
    <h4>2017.03.20 @ng-kyoto Angular Meetup #5</h4>
</div>

---
## Who?
### Name: Masui Masanori

### Twitter: [@masanori_msl](https://twitter.com/masanori_msl)
<img src="https://pbs.twimg.com/profile_images/832894991387152384/RNwtLhN5.jpg" alt="twitter" style="width: 128px; height: 128px; position: absolute; top:20%; right: 19%;"></img>

### Blog: [vaguely](http://mslgt.hatenablog.com/)

### GitHub: https://github.com/masanori840816
---
## Theme
<div style='position:absolute; left:10%; top:35%;'>
    <h3>Angularを使ってGUI部品をアニメーションさせる</h3>
</div>
---
## Sample
<a style='position:absolute; left:10%; top:40%;' href='https://github.com/masanori840816/NgGuiAnimeSample'>
    <h3>NgGuiAnimeSample - GitHub</h3>
</a>
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
]})
```
---
## Sample1 Code2
### gui-anime-sample-1.component.ts
``` xml
export class GuiAnimeSample1Component implements OnInit {
  private state: string;
  constructor() { }

  ngOnInit() {
    this.state = 'inactive';
  }
  private onButtonClicked(){
    this.state = (this.state === 'active')? 'inactive': 'active'; 
  }
  private onAnimeDone(){
    // TODO: アニメーション完了後の処理.
  }
}
```
---
## Sample1 Code3
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
## Sample1 Code4
#### このコードは大きく3つに分けられる
### 1. Trigger
アニメーション開始のきっかけを指定する。

### 2. State
アニメーションの開始前・完了後の状態を指定する。

### 3. Transition
どうアニメーションするかを指定する。

---
## Sample1 Code5
### Trigger
トリガーとして指定された"buttonState"の値が変わるとアニメーション実行
``` xml
trigger('buttonState', [
```
---
## Sample1 Code6
### State
ここでは2つの状態を設定して("inactive"、"active")、<br>
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
## Sample1 Code7
### Transition
アニメーションで切り替える状態と、<br>
どんな風にアニメーションするかを指定する。
``` xml
transition('inactive => active', animate('0ms 200ms ease-in')),
transition('active => inactive', animate('0ms 200ms ease-out'))
```
---
## Sample1 Code8
### Wildcard & Void
#### * (Wildcard)
任意の状態として扱うことができる。

複数種類の状態からひとつの状態へと切り替える場合、<br>
状態に関係なく処理したい場合などに有用。

##### 例
``` xml
transition('inactive => *', animate('0ms 200ms ease-in')),
```
#### Void
任意の状態として扱うことができる。

Wildcardとの違いは、
アニメーション対象の要素がまだ存在しない、<br>
または要素が削除される場合にも使用可能。<br>
…らしい。
---
## Sample1 Code9
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
## Sample1 Code10
### Easing1
速度を一定ではなく緩急をつけることで、<br>
より自然なアニメーションを実現する。

#### Easingの例
* [Easing Function 早見表](http://easings.net/ja)
---
## Sample1 Code11
### Easing2
※ただしCSSでは全てを選択できるわけでなく、<br>
下記から選択するかまたはベジェ曲線で指定する必要がある。

* ease （デフォルト）
* linear （はじめから終わりまで一定）
* ease-in （最初だけゆっくり）
* ease-out （最後だけゆっくり）
* ease-in-out （最初と最後だけゆっくり）
* cubic-bezier() （カスタム）

---
## Sample1 Code12
### アニメーションの実行1
Buttonにトリガーとなる"buttonState"を関連付け、<br>
クリックでその値を変更することでアニメーションを実行する。

#### gui-anime-sample-1.component.html
``` xml
<button id='sample1Button' [@buttonState]='state'
     (click)='onButtonClicked()'
     (@buttonState.done)='onAnimeDone()'>Sample1</button>
```
---
## Sample1 Code13
### アニメーションの実行2
#### gui-anime-sample-1.component.ts
``` xml
export class GuiAnimeSample1Component implements OnInit {
  private state: string;
  constructor() { }

  ngOnInit() {
    this.state = 'inactive';
  }
  private onButtonClicked(){
    this.state = (this.state === 'active')? 'inactive': 'active'; 
  }
  private onAnimeDone(){
    // TODO: アニメーション完了後の処理.
  }
}
```
---
## Sample1 Code14
### アニメーションの開始・終了イベント
アニメーション開始時、終了時に別の処理を実行したい場合、<br>
トリガーである”buttonState”を使ってイベントを取得できる

#### gui-anime-sample-1.component.html
``` xml
〜省略〜 (@buttonState.done)='onAnimeDone()'>Sample1</button>
```

#### gui-anime-sample-1.component.ts
``` xml
export class GuiAnimeSample1Component implements OnInit {
  〜省略〜 
  private onAnimeDone(){
    // TODO: アニメーション完了後の処理.
  }
}
```
---
## Sample2
### ANGULAR DOCSのボタンを真似してみる

<a style='position:absolute; left:25%; top:35%;' href='https://angular.io/docs/ts/latest/'>
Angular Docs - ts - INDEX - Angular
</a>
---
## Sample2
### やりたいこと
マウスオーバー時にアニメーションを実行し、<br>
ボタンからマウスカーソルが離れたら元に戻す。

#### マウスオーバー時
* 枠線の色を変える
* 影の大きさを変える
* ボタンの位置を変える
---
## Sample2 Code1
#### gui-anime-sample-2.component.ts
``` xml
import { Component,Input,trigger,state,style,
  transition,animate,OnInit} from '@angular/core';
import { SampleButton } from '../sample-button';
@Component({
  selector: 'app-gui-anime-sample-2',
  templateUrl: './gui-anime-sample-2.component.html',
  styleUrls: ['./gui-anime-sample-2.component.css'],
  animations: [
    trigger('buttonState', [
      state('active', style({
        border: '3px solid #2196F3',
        boxShadow: '0px 5px 6px 3px #CECECE',
        transform: 'translate3d(-1px, -1px, 50px) scale(1.02)',
        zIndex: 30
      })),
      state('inactive',   style({
        border: '1px solid #CECECE',
        boxShadow: '0px 1px 3px 1px #CECECE',
        transform: 'translate3d(0px, 0px, 0px) scale(1)',
        zIndex: 10
      })),
      transition('active => inactive', animate('200ms ease-in')),
      transition('inactive => active', animate('200ms ease-out'))
    ])
```
---
## Sample2 Code2
#### gui-anime-sample-2.component.ts
``` xml
  ]
})
export class GuiAnimeSample2Component implements OnInit {
  private sampleButtons: SampleButton[] = [
    {id: 0, pageTitle: 'Page1', discription: 'Page No.1', state: 'inactive'},
    {id: 1, pageTitle: 'Page2', discription: 'Page No.2', state: 'inactive'},
    {id: 2, pageTitle: 'Page3', discription: 'Page No.3', state: 'inactive'},
  ];
  constructor() { }

  ngOnInit() {
  }
  private onButtonIn(targetId: number){
    this.sampleButtons[targetId].state = "active";
  }
  private onButtonOut(targetId: number){
    this.sampleButtons[targetId].state = "inactive";
  }
}
```
---
## Sample2 Demo

---
## Pause & Cancel
アニーションを途中で止めたり<br>
キャンセルする方法を探すも見つけられずorz

ただし、今回のアニメーションはAngular独自のものではなく、<br>
CSSのアニメーションを利用しているので、<br>
方法はあるはず。

[AnimationPlayer](https://angular.io/docs/ts/latest/api/core/index/AnimationPlayer-class.html)を使えば解決できそう…？

---
## What's new in ver.4.0?
### @angular/coreからの分割
アニメーションを使うときはimport文が別れて不便？だが、<br>
使わない時に不要なファイルが含まれなくなるメリットがある。

### transitionでの関数サポート
transitionの”inactive => active”を、<br>
アニメーション実行前の状態、実行後の状態を引数に取り、<br>
bool値を返す関数に置き換えることが可能に。

---
## Impressions
#### ・ 思った以上に簡単にアニメーション出来てすごい!(こなみかん)
#### ・ アニメーションをインスタンスとして扱えると便利そう。<br>(AnimationPlayerだと解決できる…?)
---
## References
* [Animations - ts - GUIDE - Angular](https://angular.io/docs/ts/latest/guide/animations.html)
* [ng2-hands-on-seed
 - ng-japan - GitHub](https://github.com/ng-japan/ng2-hands-on-seed/tree/master/courses/tutorial)
* [[Angular 4.0] router/http/animationsモジュールの更新について | <output type="laco">](https://blog.lacolaco.net//post/ng4-feature-libs-update/)
* [
〜なんだかいけそうな気がする〜Angular入門 - Speaker Deck](https://speakerdeck.com/kozake/nandakaikesounaqi-gasuru-angularru-men)
* [Angular2 animation - Qiita](http://qiita.com/kawakami-kazuyoshi/items/639cbe55da4cd1c1df80)
* [Episode 103: ngAir 103 - Animation in Angular with Matias Niemela - AngularAir](http://ngair.io/ep103-2017)
* [Angular 2 Cookbook](http://shop.oreilly.com/product/9781785881923.do)
---
<div style='position:absolute; left:25%; top:35%;'>
    <h1>Have a nice day!</h1>
</div>