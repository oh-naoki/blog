---
title: "PorterDuff を使って角丸プログレスバーを作る"
date: 2020-06-07T08:03:20+09:00
draft: false
---

2つの画像を合成するときに使える PorterDuff を利用して角丸プログレスバー作りました

### PorterDuff とは
2つの画像を組み合わせるときのいくつかのルールを提供している便利なスキーマみたいなものです(雑な理解)
- https://developer.android.com/reference/android/graphics/PorterDuff.Mode

例えば、大きさの異なる2つの円を用意して PorterDuff を使えばドーナッツみたいにくり抜くこともできます。

![porter_duff_image](https://user-images.githubusercontent.com/16508442/83962207-84a38c80-a8d6-11ea-8052-6a72872a498f.png)

### PorterDuff を利用して角丸プログレスバーを作る
Githubで検索すると角丸プログレスバーはいくつかでてくるのですが、両端が角丸になっているものが多かったので片側(進行方向)は垂直に切れているプログレスバーを作ってみることにしました。

<!-- {{< figure src="https://user-images.githubusercontent.com/16508442/83962407-0811ad80-a8d8-11ea-8d1c-24a243a049b3.gif" width="184" height="92" >}} -->

手順はまず初めに背景となる角丸の Retangle を描画してその上にプログレスバーとなる角丸ではない Rectangle を描画します。
描画する際に使用する Paint に重ね合わせるときのルールを指定することで背景画像に対してどう切り取るか、描画順序はどうするかを決めることができます。

![porter_duff_draw_rectangle](https://user-images.githubusercontent.com/16508442/83963379-ac97ed80-a8e0-11ea-9e57-9a13abc71863.png)

```kt
    leftBarPaint = Paint().apply {
        color = leftColor
        xfermode = PorterDuffXfermode(PorterDuff.Mode.SRC_IN)
    }

    rightBarPaint = Paint().apply {
        color = rightColor
        xfermode = PorterDuffXfermode(PorterDuff.Mode.SRC_IN)
    }

    backgroundPaint = Paint().apply {
        color = Color.BLACK
    }

    canvas.drawRoundRect(
        0F,
        0F,
        this.width.toFloat(),
        this.height.toFloat(),
        radius.px(),
        radius.px(),
        backgroundPaint
    )

    canvas.drawRoundRect(
        progressCurrentWidth.toFloat(),
        0F,
        this.width.toFloat(),
        this.height.toFloat(),
        0F,
        0F,
        rightBarPaint
    )

    canvas.drawRoundRect(
        0F,
        0F,
        progressCurrentWidth.toFloat(),
        this.height.toFloat(),
        0F,
        0F,
        leftBarPaint
    )
```

### まとめ
複雑な図形も PorterDuff を使えば簡単に描画できるケースもあることが分かりました。
ソースコードは Github に載せているので何かの助けになれば幸いです

https://github.com/oh-naoki/CustomProgress