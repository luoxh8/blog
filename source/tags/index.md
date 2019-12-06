<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
</head>
<body>
<script>
    const categories = ["Python", "Django"]
    for (const element of categories) {
        const a = document.createElement("a")
        a.href = element
        a.innerText = element
        a.style.fontSize = `${Math.floor(Math.random() * (24 - 16 + 1) + 16)}px`
        a.style.textDecoration = 'none'
        a.style.marginLeft = '1rem'
        a.style.color = (() => {
                return '#' +
                    (function (color) {
                        return (color += '0123456789abcdef' [Math.floor(Math.random() * 16)]) &&
                            (color.length == 6) ? color : arguments.callee(color);
                    })('');
        })()
        document.getElementsByClassName('article-container')[0].appendChild(a)
    }
</script>
</body>
</html>
