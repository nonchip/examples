<!DOCTYPE html>
<html>

<head>
<title>mdown reader</title>

<script src="lib/jquery.min.js"></script>
<script src="lib/marked.min.js"></script>
<script src="lib/prettify.min.js"></script>
<script src="lib/strapdown.js"></script>
<script src="lib/params.js"></script>
<link rel="stylesheet" href="lib/strapdown.css" />
<link rel="stylesheet" href="lib/mdown-style.css" />
</head>
<body>

<xmp id="markdown" theme="" style="display:none;"></xmp>
<h1 id="loading">Loading...</h1>

<script>

// set marked options
marked.setOptions({
  renderer: new marked.Renderer(),
  gfm: true,
  tables: true,
  breaks: false,
  pedantic: false,
})

function render(path) {
  var xmp = document.getElementById("markdown");

  // set theme first.
  var theme = getParameterByName("theme")
  if (theme && theme && theme.length > 0) {
    $(xmp).attr('theme', theme)
  }else{
    $(xmp).attr('theme', 'readable');
  }

  $.get(path, function(data) {
    xmp.innerHTML = data;
    strapdown(window, document)

    // fix the navbar
    $('.navbar-fixed-top')
      .addClass('navbar-static-top')
      .removeClass('navbar-fixed-top')

    var title=$(".container h1")[0].innerHTML
    if (title && title && title.length > 0) {
      document.getElementsByTagName('title')[0].innerHTML=title
      $('.navbar-inner #headline.brand')[0].innerHTML=title+'<tt style="font-size:0.7em;margin-left:5em">'+path+'</tt>'
      $(".container h1").hide()
    }

    $('#loading').hide()
  })
}

var hash = window.location.hash.substring(1)
if (hash.length > 0) {
  if(hash.substring(hash.lenght - 1) == "/"){
    window.location.hash = window.location.hash+"index.md"
    window.location.reload()
  }
  render(hash)
} else {
  window.location.hash = "#index.md"
  window.location.reload()
}

window.onhashchange = function(){
  window.location.reload()
}

</script>
</body>
</html>
