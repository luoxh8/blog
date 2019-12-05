<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
</head>
<body>
<script>
    const categories = ["后端开发", ]
    for (const element of categories) {
        const a = document.createElement("a")
        a.href = element
        a.innerText = element
        a.style.fontSize = `${Math.floor(Math.random() * (24 - 16 + 1) + 16)}px`
        a.style.textDecoration = 'none'
        a.style.marginLeft = '1rem'
        document.getElementsByClassName('article-container')[0].appendChild(a)
    }
</script>
</body>
</html>
