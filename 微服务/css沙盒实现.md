# css沙盒实现-shadowDom
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div>
        <p>hello world</p>
        <div id="shadow"></div>
    </div>

    <script>
        let shadowDom = shadow.attachShadow({mode: 'closed'}); //外界无法访问
        let pElm = document.createElement('p');
        pElm.innerText = 'hello lee';
        let styleElm = document.createElement('style');
    
        styleElm.textContent = `
            p{color: red}
        `;
    
        shadowDom.appendChild(styleElm);
        shadowDom.appendChild(pElm);
    </script>
```
