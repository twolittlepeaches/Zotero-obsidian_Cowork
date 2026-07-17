---
cssclasses:
  - zotero-note
status: todo
weight: 1
field:
date: "{% if date %}{{ date | format("YYYY-MM") }}{% endif %}"
DOI: "{% if DOI %}{{ DOI }}{% endif %}"
tags:
  - 文献笔记
  - 论文阅读
{%- if allTags %}
{%- for tag in allTags.split(",") %}
{%- set cleanTag = tag.trim().split(" ").join("_").split("/").join("_").split("#").join("").split("，").join("_") %}
{%- if cleanTag %}
  - {{ cleanTag }}
{%- endif %}
{%- endfor %}
{%- endif %}
zotero_tags: "{% if allTags %}{{ allTags }}{% endif %}"
authors: "{% for t in creators %}{{ t.firstName }} {{ t.lastName }}{{ t.name }}{% if not loop.last %}, {% endif %}{% endfor %}"
期刊简称: "{% if journalAbbreviation %}{{ journalAbbreviation }}{% endif %}"
languages: "{{ language }}"
类别: "{{ itemType }}{% if thesisType %} {{ thesisType }}{% endif %}"
期刊: "{% if publicationTitle %}{{ publicationTitle }}{% endif %}{% if university %} {{ university }}{% endif %}"
---

## {{ title }}

## 文献信息

| 字段 | 内容 |
| --- | --- |
| 标题 | {{ title }} |
| 作者 | {% for t in creators %}{{ t.firstName }} {{ t.lastName }}{{ t.name }}{% if not loop.last %}, {% endif %}{% endfor %} |
{% if date %}| 年份 | {{ date | format("YYYY") }} |
{% endif %}{% if DOI %}| DOI | {{ DOI }} |
{% endif %}{% if allTags %}| Zotero 标签 | {{ allTags }} |
{% endif %}{% if publicationTitle or university %}| 期刊/会议 | {% if publicationTitle %}{{ publicationTitle }}{% endif %}{% if university %} {{ university }}{% endif %} |
{% endif %}{% if itemType or thesisType %}| 类型 | {{ itemType }}{% if thesisType %} {{ thesisType }}{% endif %} |
{% endif %}{% if callNumber %}| Call Number | {{ callNumber }} |
{% endif %}{% if pdfLink %}| PDF | {{ pdfLink }} |
{% endif %}{% if pdfZoteroLink %}| Zotero | {{ pdfZoteroLink }} |
{% endif %}{% if url %}| URL | [Open online]({{ url }}) |
{% endif %}
## 摘要

{% if abstractNote %}{{ abstractNote }}{% else %}_Zotero 中暂无摘要。_{% endif %}

## 我的笔记

{% persist "research_v1" %}

{% endpersist %}

## 批注

{% macro annotationLabel(type, color) %}{% if type == "highlight" %}<mark style="background-color: {{ color }}">Highlight</mark>{% endif %}{% if type == "text" %}Note{% endif %}{% if type == "image" %}Image{% endif %}{% if type == "ink" %}Ink{% endif %}{% endmacro %}

{% persist "annotations" %}
{% set newAnnotations = annotations | filterby("date", "dateafter", lastImportDate) %}
{% if newAnnotations.length > 0 %}

### Imported: {{ importDate | format("YYYY-MM-DD HH:mm") }}

{% for each in newAnnotations %}

{{ annotationLabel(each.type, each.color) }}{% if each.page %} 第 {{ each.page }} 页{% endif %} [jump to](zotero://open-pdf/library/items/{{ each.attachment.itemKey }}?page={{ each.page }}&annotation={{ each.id }})

{% if each.annotatedText %}> {{ each.annotatedText }}
{% endif %}{% if each.comment %}
标注：{{ each.comment }}
{% endif %}{% if each.imageBaseName %}
![[{{ each.imageBaseName }}]]
{% endif %}
{% endfor %}

{% endif %}
{% endpersist %}