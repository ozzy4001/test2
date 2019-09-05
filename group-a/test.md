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
(function ($, global, GsBase) {
    if (GsBase.device == 'pc') {
        $(document).on('focusin', '.gui-btn', function(){
            $(this).addClass('focus')
        })
        .on('focusout mouseleave', '.gui-btn', function(){
            $(this).removeClass('focus')
        });

        // IE 버튼 active 
        if (GsBase.browserName == 'ie') {
            $(document).on('mousedown', '.gui-btn', function(){
                $(this).addClass('active')
            })
            .on('mouseup', '.gui-btn', function(){
                $(this).removeClass('active')
            });
        }
    }
}) (jQuery, window, window.GsBase = window.GsBase || {});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

