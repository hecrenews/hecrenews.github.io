---
layout: default
title: "Hecrenews | Search"
description: "Search the Hecrenews for an article"
---
<h1 style="display:flex;justify-content:center">Search</h1>
<form onSubmit="return search()">
  <input type="text" id="search-query" value="News" />
  <button type="submit" value="Search"><i class="fas fa-search fa-2x"></i></button>
</form>
<div class="tag-container">
</div>

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
      </li>
      {% endfor %}
    </ul>
  </div>
</div>

<script type="text/javascript">

  postTags = [];
  allTags = [];
  for (var i = 0; i < $('.post-list-item').length; i++) {
    var tags = $($('.post-list-item')[i]).attr('tags').split(" ");
    var realTags = []
    for (var j = 0; j < tags.length; j++) {
      if (tags[j] === "\n" || tags[j] === "") {
      } else {
        realTags.push(tags[j]);
        if (!allTags.includes(tags[j])) {
          allTags.push(tags[j]);
        }
      }
    }
    postTags.push(realTags);
  }
  allTags.sort();

  for (var i = 0; i < allTags.length; i++) {
    $('.tag-container').append("<p class='tag' onclick='search(\"" + allTags[i] + "\")'>" + allTags[i] + "</p>");
  }

  function search(query) {
    $('.post-list-item').addClass('hidden');
    if (!query)
    {
      if (!$("input#search-query").val()) {
        return false;
      }
      query = $("input#search-query").val();//.toLowerCase();
      query = query[0].toUpperCase() + query.slice(1);
    }
    else {
      $("input#search-query").val(query);
    }

    for (var i = 0; i < $('.post-list-item').length; i++) {
      if (postTags[i].includes(query) ||
          $($('.post-list-item')[i]).find('.post-list-link').html().includes(query)) {
        $($('.post-list-item')[i]).removeClass('hidden');
      }
    }

    return false;
  }
</script>

<style>
  form {
    display: flex;
    justify-content: center;
  }

  form input[type=text] {
    margin-right: 16px;
    padding: 8px;
    border-radius: 16px;
    border: 1px solid #ccc;
  }

  form button[type=submit] {
    padding: 0;
    border-radius: 50%;
    border: none;
    background: transparent;
  }

  .search-results {
    padding: 16px;
  }

  .hidden {
    display: none;
  }

  .tag-container {
    display: flex;
    flex-wrap: wrap;
    padding: 16px;
  }

  .tag {
    padding: 8px;
    background-color: #ff4d4d;
    color: #fff;
    box-shadow: 0 1px 3px rgba(0,0,0,.25);
    font-size: .8rem;
    margin: 8px 8px 0 0;
    cursor: pointer;
  }
</style>
