Chrome:

Add this to css:

    touch-action: none;

Safari:

Add this JS

    var _disable = function (e) {
      e.preventDefault();
    };

    $j('div[data-role="fixed-flow-menu"]').bind('touchmove', _disable);
