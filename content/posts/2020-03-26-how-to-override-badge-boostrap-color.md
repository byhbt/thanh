---
categories:
  - Frontend
tags:
  - scss
  - bootstrap
title: How to override Bootstrap badge color
date: 2020-03-26
---

Html structure

```slim
td.sm-down-hidden
  - if payment?
    span.badge.badge-pill.badge-payment--verified = 'verified'
  - else
    span.badge.badge-pill.badge-payment--pending = 'pending'
```

What does not work

```scss
.badge-payment {
  &--verified {
    background: $verified-background;
    color: $verified-color;
  }

  &--pending {
    background: $pending-background;
    color: $pending-color;
  }
}

```

What work

```scss
.badge-pill {
  &.badge-payment {
    &--verified {
      background: $verified-background;
      color: $verified-color;
    }

    &--pending {
      background: $pending-background;
      color: $pending-color;
    }
  }
}

```

## What is BEM?

[https://css-tricks.com/bem-101/](https://css-tricks.com/bem-101/)

## What are the Building Blocks?

[https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)