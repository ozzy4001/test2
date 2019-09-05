# menuA

{% code-tabs %}
{% code-tabs-item title="markup" %}
```markup
<button class="gui-btn"><strong>Button</strong></button><!-- Case: strong 태그 사용 -->
<button class="gui-btn bold"><span>Button</span></button><!--Case: 클래스 bold 추가 -->
```
{% endcode-tabs-item %}

{% code-tabs-item title="js" %}
```javascript
/* –––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
		
	개발중
	
–––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––– */
;(function(doc, win, gs) {
	"use strict";

	var Overlay = function (){};
	var doc = document.querySelector( 'body' );
	var startY = 0;
	var scrollBlank = gs.browserName === 'ie'? 17 : 8;

	window.addEventListener('touchstart', function(e) {
		startY = e.touches[0].clientY;
	}, false);
	
	
	Overlay.prototype.constructor = Overlay;
	
	Overlay.prototype = {
		
		open: function( t ) { 
			const self = this;
			const modal = document.querySelector( t );
			const scrollView = document.querySelector( '.md-content main' );

			if (doc.scrollHeight > win.innerHeight && gs.device =='pc') {
				$(doc).css({marginRight: scrollBlank });
			}

			doc.classList.add( 'open-overlay' );
			modal.classList.add('on');
			
			modal.addEventListener('touchmove', function(e) {
				e.preventDefault();
				e.stopPropagation();
			}, false);

			setTimeout (function(){
				scrollView
					.addEventListener('touchmove', self.stopBubbleScroll, false);
			}, 300);
		}, 
		close: function( t ) {
			var modal = document.querySelectorAll( t || '.overlay.on' );

			for ( var i = 0; i < modal.length; i++ ) {
				modal[i].classList.remove('on');
			}

			setTimeout (function(){
				doc.classList.remove('open-overlay');
				$(doc).removeAttr('style');
			}, 300);
		},
		// ios 터치 스크롤 체크
		stopBubbleScroll: function(e){
			
			const domH = this.clientHeight;
			const scrollH = this.scrollHeight;
			const scrollY = parseInt( this.scrollTop );
			const moveY = parseInt( e.touches[0].clientY - startY );
		
			if ( scrollH > domH ) {
				if ((moveY < 0 && scrollY == 0) 
					|| (moveY < 0 && scrollY <= scrollH - domH && scrollY + domH < scrollH)
					|| (moveY > 0 && scrollY > scrollH - domH && scrollY + domH > scrollH)
					|| (moveY > 0 && scrollY > 0 && scrollY + domH <= scrollH)
				){
					e.stopPropagation();
				} 
			}
		}
	};
		
	gs.overlay = new Overlay();


})(document, window, window.gs = window.gs || {});

```
{% endcode-tabs-item %}

{% code-tabs-item title="scss" %}
```css

// 방향, 각도
//––––––––––––––––––––––––––––––––
@function convert-angle ( $value, $unit ) {
	
	$convertable-units: deg grad turn rad;
	$conversion-factors: 1 (10grad/9deg) (1turn/360deg) (3.1415926rad/180deg);
	
	@if index( $convertable-units, unit($value)) and index($convertable-units, $unit ) {
		@return $value
			/ nth($conversion-factors, index( $convertable-units, unit($value )))
			* nth($conversion-factors, index( $convertable-units, $unit ));
	}
	@warn "Cannot convert `#{unit($value)}` to `#{$unit}`.";
}
// 방향 유무
//––––––––––––––––––––––––––––––––
@function is-direction ( $value ) {
	$is-direction: index((to top, to top right, to right top, to right, to bottom right, to right bottom, to bottom, to bottom left, to left bottom, to left, to left top, to top left), $value);
	$is-angle: type-of($value) == 'number' and index('deg' 'grad' 'turn' 'rad', unit($value));
	@return $is-direction or $is-angle;
}

// webkit 방향 설정
//––––––––––––––––––––––––––––––––
@function legacy-direction ( $value ) {
	@if is-direction($value) == false {
		@warn "Cannot convert `#{$value}` to legacy syntax because it doesn't seem to be an angle or a direction";
	}
  
	$conversion-map: ( 
		to top				: bottom,
		to top right		: bottom left,
		to right top		: left bottom,
		to right			: left,
		to bottom right	: top left,
		to right bottom	: left top,
		to bottom		: top,
		to bottom left	: top right,
		to left bottom	: right top,
		to left				: right,
		to left top		: right bottom,
		to top left		: bottom right
	);

	@if map-has-key($conversion-map, $value) {
		@return map-get( $conversion-map, $value );
	}
	@return 90deg - convert-angle($value, 'deg');
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

