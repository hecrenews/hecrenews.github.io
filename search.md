---
layout: default
title: "Hecrenews | Search"
description: "Search the Hecrenews for an article"
---

This is very basic, don't expect (still getting upgraded)

<form onSubmit="return search()">
  <input type="text" id="search-query" value="News" />
  <input type="submit" value="Search" />
</form>

<div class="search-results">
  <div class="post-list-container container">
    <ul class="post-list">
      {% for post in site.posts %}
      <li class="post-list-item hidden" thumb="{{ post.thumb }}" tags="
      {%for tag in post.tags%}{{ tag }} {% endfor %}">
        <div class="post-list-info">
          <a class="post-list-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
          <span class="post-list-date">{{ post.date | date: "%B %-d, %Y" }}</span>
          <div class="post-tag-container">
            {% for tag in post.tags %}
            <p class="post-tag">{{ tag }}</p>
            {% endfor %}
          </div>
        </div>
        <div class="post-list-tool-tip close">
          <div class="post-list-tool-tip-img"></div>
          <!-- Later figure out how to make this load dynamically -->
          <div class="post-list-tool-tip-txt">{{ post.excerpt }}</div>
        </div>
      </li>
      {% endfor %}
    </ul>
  </div>
</div>

<script type="text/javascript">

  postTags = [];
  for (var i = 0; i < $('.post-list-item').length; i++) {
    var tags = $($('.post-list-item')[i]).attr('tags').split(" ");
    var realTags = []
    for (var j = 0; j < tags.length; j++) {
      if (tags[j] === "\n" || tags[j] === "") {
      } else {
        realTags.push(tags[j]);
      }
    }
    postTags.push(realTags);
  }
  console.log(postTags);

  function search() {
    if (!$("input#search-query").val())
      return false;
    $('.post-list-item').addClass('hidden');
    var query = $("input#search-query").val();//.toLowerCase();
    query = query[0].toUpperCase() + query.slice(1);

    for (var i = 0; i < $('.post-list-item').length; i++) {
      if (postTags[i].includes(query)) {
        $($('.post-list-item')[i]).removeClass('hidden');
      }
    }

    return false;
  }
</script>

<style>
  .hidden {
    display: none;
  }
</style>
