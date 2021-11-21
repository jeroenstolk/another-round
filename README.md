The used API: https://punkapi.com/documentation/v2

# helpful resources

## Color scheme used
- https://www.schemecolor.com/summer-theme-color-scheme.php

## Helping me sort my API calls:
- https://stackoverflow.com/a/66394427
- https://svelte.dev/repl/5c95e18702764aefa71ff2b4616a6c6e?version=3.20.1

## Image upload:
- https://svelte.dev/repl/596d671cba1942f49184618d439b243c?version=3.44.1

## Google font
- https://fonts.google.com/specimen/Roboto+Slab

```bash
useful code goes here
and here
```

## snippets

- https://www.javascripture.com/FileReader
```
<input type='file' accept='image/*' onchange='openFile(event)'><br>
<img id='output'>
<script>
  var openFile = function(event) {
    var input = event.target;

    var reader = new FileReader();
    reader.onload = function(){
      var dataURL = reader.result;
console.log(dataURL);
    };
    reader.readAsDataURL(input.files[0]);
  };
</script>
```

- https://reqbin.com/