---
layout: post
title:  "OLWLG Vertical Bookmarklet"
date:   2015-04-23 15:00:00
categories: olwlg bgg
---

To get the OLWLG Bookmarklet please favourite the below link, or drag it to your bookmark toolbar.

<a href="javascript:(function(){(function(){var%20rows=[];var%20rowsList=document.getElementById('tablebody').getElementsByTagName('tr');for(var%20i=0;i%20%3C%20rowsList.length%20-%201;++i){var%20valueElems=rowsList[i].getElementsByClassName(%22value%22);rows[i]={'tr':rowsList[i]};for(var%20j=0;j%20%3C%20valueElems.length;++j){if(valueElems[j].type=='text'){rows[i].val=parseFloat(valueElems[j].value);}}}function%20setCheckbox(y,x,on){rows[y].tr.getElementsByTagName('td')[x+2].getElementsByTagName(%22input%22)[0].checked=on%3F'checked':'';}var%20headers=document.body.getElementsByClassName(%22vertical%22);var%20mygamevalues=[];for(var%20i=0;i%20%3C%20headers.length%20/2;++i){var%20div=document.createElement('div');var%20tb=document.createElement('input');tb.style.width='3em';tb.onkeyup=(function(){var%20t=tb;var%20offset=i;return%20function(){console.log(%22t.val%20=%20%22+t.value);var%20val=parseFloat(t.value);for(j=0;j%3Crows.length;++j){setCheckbox(j,offset,(rows[j].val%20%3E=val));}};})();headers[i].parentNode.appendChild(div);div.appendChild(tb);mygamevalues[i]={'tb':tb};}})()})();">OLWLG Vertical</a>

If someone can point me in the direction of the author of this I would love to credit them.