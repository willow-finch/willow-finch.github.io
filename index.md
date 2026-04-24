---
layout: default
title: Home
---

我是**柳小鸟**. 有大纲, 不烂尾, 但是经常修改, 请等完结再看.

<hr class="divider">
<p></p>

# 最近更新 
{% assign all_books = site.books | sort: "date" | reverse %}
{% assign filtered_books = "" | split: "" %}

{% for book in all_books %}
  {% comment %}
  Get the path relative to _books folder
  Example: 
  - "_books/book1.md" -> "book1.md" (no subdirectory)
  - "_books/book1/01_chapter1.md" -> "book1/01_chapter1.md" (has subdirectory)
  {% endcomment %}
  {% assign path_without_books = book.relative_path | remove_first: "_books/" %}
   
  {% comment %}Check if the remaining path contains a slash (means it's in a subdirectory){% endcomment %}
  {% if path_without_books contains "/" %}
    {% assign filtered_books = filtered_books | push: book %}
  {% endif %}
{% endfor %}

<!-- Debug output
<pre style="background: #f0f0f0; padding: 10px; border: 1px solid #ccc; overflow: auto;">
Filtered Books ({{ filtered_books | size }} items):
{% for book in filtered_books %}
  Path: {{ book.relative_path }}
  Title: {{ book.title }}
  Date: {{ book.date }}
  ---
{% endfor %}
</pre> -->

{% assign latest_posts = filtered_books %}

<div class="latest-updates-block">
  {% for post in latest_posts limit:5 %}
    {% assign book_info = site.data.books_info[post.book] %}
    <div class="update-item">
      <span class="update-date">{{ post.date | date: "%Y-%m-%d" }}</span>
      <span class="tab-space"></span>
      <a href="{{ post.url }}">
        <!-- &lt;&lt;{{ book_info.title }}&gt;&gt; - {{ post.title }} -->
        &lt;&lt;{{ book_info.title }}&gt;&gt; - 第{{ post.order }}章: {{ post.title }}
      </a>
    </div>
  {% endfor %}
</div>

<hr class="divider">
<p></p>

# 全部书目列表

{% assign books = site.data.books_info %}

<div class="book-block">
  {% for book in books %}
    <a class="book-card" href="/books/{{ book[0] }}/">
      <h2 class="book-title">{{ book[1].title }}</h2>
      <p class="book-description" style="white-space: pre-line;">{{ book[1].description }}</p>
    </a>
  {% endfor %}
</div>
