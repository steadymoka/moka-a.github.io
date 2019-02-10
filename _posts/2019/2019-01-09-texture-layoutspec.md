---
layout: post
title: "iOS | About layoutSpec in texture"
author:
published: false
modified:
categories: ios
excerpt: About layoutSpec in texture
tags: [ios, swift, texture, layoutspec]
image:
  feature:
date: 2019-1-9
---

## ASOverlayLayoutSpec
[https://texturegroup.org/docs/layout2-layoutspec-types.html#asoverlaylayoutspec](https://texturegroup.org/docs/layout2-layoutspec-types.html#asoverlaylayoutspec)

>ASOverlayLayoutSpec lays out its child (blue), stretching another component on top of it as an overlay (red).

>The overlay spec’s size is calculated from the child’s size. In the diagram below, the child is the blue layer. The child’s size is then passed as the constrainedSize to the overlay layout element (red layer). Thus, it is important that the child (blue layer) must have an intrinsic size or a size set on it.

ASOverlayLayoutSpec은 child 의 요소 (파란색)를 배치하고 다른 요소를 오버레이(빨간색)로 겹쳐서 배치합니다.

ASOverlayLayoutSpec의 크기는 child 크기에서 계산됩니다. 아래 다이어그램에서 child 는 파란색 레이어입니다. child의 크기는 constrainedSize로 overlay의 node(요소)로 (빨간색 레이어)에 전달됩니다. 따라서 중요한 것이 child(파란색)가 본질적인 크기 또는 크기가 설정되어 있어야합니다.

## ASBackgroundLayoutSpec
[https://texturegroup.org/docs/layout2-layoutspec-types.html#asbackgroundlayoutspec](https://texturegroup.org/docs/layout2-layoutspec-types.html#asbackgroundlayoutspec)

>ASBackgroundLayoutSpec lays out a component (blue), stretching another component behind it as a backdrop (red).

>The background spec’s size is calculated from the child’s size. In the diagram below, the child is the blue layer. The child’s size is then passed as the constrainedSize to the background layout element (red layer). Thus, it is important that the child (blue layer) must have an intrinsic size or a size set on it.

중요한 내용은, layoutSpec 의 크기가 child 에 의해서 결정이 된다는 것이다. 아무리 background의 node 의 크기를 설정해주어도 child의 크기에 맞춰진다.

## ASCenterLayoutSpec
[https://texturegroup.org/docs/layout2-layoutspec-types.html#ascenterlayoutspec](https://texturegroup.org/docs/layout2-layoutspec-types.html#ascenterlayoutspec)

>ASCenterLayoutSpec centers its child within its max constrainedSize.

>If the center spec’s width or height is unconstrained, it shrinks to the size of the child. ASCenterLayoutSpec has two properties:

child를 부모안에서 최대 중앙으로 배치 합니다. 