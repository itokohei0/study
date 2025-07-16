<style>
    body {
      counter-reset: chapter;
    }
    h1 {
        counter-reset: sub-chapter;
    }
    h2 {
        counter-reset: section;
    }

    h1::before {
        counter-increment: chapter;
    }
    h2::before {
        counter-increment: sub-chapter;
        content: counter(sub-chapter) "章. ";
    }
    h3::before {
        counter-increment: section;
        content: counter(sub-chapter) "-" counter(section) ". ";
    }
</style>

# 達人に学ぶSQL徹底指南書

## 魔法のSQL

@import "chap1/sec1.md"

<div style="page-break-before:always"></div>

@import "chap1/sec2.md"

<div style="page-break-before:always"></div>

@import "chap1/sec3.md"

<div style="page-break-before:always"></div>

@import "chap1/sec4.md"

<div style="page-break-before:always"></div>

@import "chap1/sec5.md"

<div style="page-break-before:always"></div>

@import "chap1/sec6.md"

<div style="page-break-before:always"></div>

@import "chap1/sec7.md"

<div style="page-break-before:always"></div>

@import "chap1/sec8.md"

<div style="page-break-before:always"></div>

@import "chap1/sec9.md"

<div style="page-break-before:always"></div>

@import "chap1/sec10.md"

<div style="page-break-before:always"></div>

@import "chap1/sec11.md"

<div style="page-break-before:always"></div>

@import "chap1/sec12.md"

<div style="page-break-before:always"></div>

## RDBの世界

@import "chap2/sec13.md"

<div style="page-break-before:always"></div>

@import "chap2/sec14.md"

<div style="page-break-before:always"></div>

@import "chap2/sec15.md"

<div style="page-break-before:always"></div>

@import "chap2/sec16.md"

<div style="page-break-before:always"></div>

@import "chap2/sec17.md"

<div style="page-break-before:always"></div>

@import "chap2/sec18.md"

<div style="page-break-before:always"></div>

@import "chap2/sec19.md"

<div style="page-break-before:always"></div>

@import "chap2/sec20.md"

<div style="page-break-before:always"></div>

@import "chap2/sec21.md"

<div style="page-break-before:always"></div>

@import "chap2/sec22.md"

<div style="page-break-before:always"></div>

@import "chap2/sec23.md"