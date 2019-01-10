---
title: ionic issue 01
date: 2019-01-01 15:20:32
tags:
- 문제해결
- ionic
---

## 이슈

```javascript
async mapHeightInit(){
  if(!this.isPlaceSearch) return false;

  let contentEle:HTMLElement = await this.content.getScrollElement();
  this.mapFullSize = contentEle.clientHeight - 100 + "px";
  this.mapDiv.nativeElement.style.height = this.mapFullSize;
}
```

화면에 전체높이를 알아야 가능한 작업이 있어서 Ionic의 Content를 가져와서 height 값을 계산해서 특정 Element의 높이를 정해줫다.

문제는 Angular Animation을 이용한 높이를 재조정 이후에 Height값을 다시 지정하는게 어려웠다. 높이를 재설정 해줫지만 적용되지 않았다.

## 문제점

Angular Animation을 이용해서 높이값이 재조정 될 때 transition의 time이 300ms 였고, 모든 이벤트가 끝난 이후에 높이를 설정해줘야 했다.

```javascript
export function HeightToggle(to, name="HeightToggle") {
	return trigger(name,[
		state('1', style({height: '*'})),
		state('0', style({height: '{{ to }}'}), {params: {to}}),
		transition('1 => 0', animate('300ms ease-in')),
		transition('0 => 1', animate('300ms ease-out'))
	]);
}
```

## 해결

다행히 Angular에서는 Animation이 끝나는 이벤트가 존재했다.

```html
<div class="toggle" [@toggle]="show" 
  (@toggle.start)="animationStarted($event)"
  (@toggle.done)="animationDone($event)"
*ngIf="show">
```