---
layout: post
title:  "Django: multiple choices in django admin list_filter"
categories: django
---


While using django admin `list_filter`, it only support one select.

For using multiple choices, you can modify URL.


```
?status__exact=a

?status__in=a%2Cb
```

You can filter by using `,`(comma).

`%2C` equals to `,` (comma)


And have to add UI cause django admin not support multiple UI. 

So I customize `change_list.html` from django admin (for my case, I use `suit` for admin, so customize `change_list.html` in suit)

{% raw %}
```html
{% extends "admin/base_site.html" %}
{% load i18n admin_static admin_list admin_urls suit_list suit_tags %}
{% load url from suit_compat %}

{% block extrastyle %}
  {{ block.super }}
  {#  <link rel="stylesheet" type="text/css" href="{% static "admin/css/changelists.css" %}" />#}
  {% if cl.formset %}
    {#    <link rel="stylesheet" type="text/css" href="{% static "admin/css/forms.css" %}" />#}
  {% endif %}
  {% if cl.formset or action_form %}
    {% url 'admin:jsi18n' as jsi18nurl %}
    <script type="text/javascript" src="{{ jsi18nurl|default:'../../jsi18n/' }}"></script>
  {% endif %}
  {{ media.css }}
  {% if not actions_on_top and not actions_on_bottom %}
    <style>
      {#      #changelist table thead th:first-child {width: inherit}#}
    </style>
  {% endif %}
{% endblock %}

{% block extrahead %}
  {{ block.super }}
  {{ media.js }}
  {% if action_form %}{% if actions_on_top or actions_on_bottom %}
    <script type="text/javascript">
      (function ($) {
        $(document).ready(function ($) {
          $("tr input.action-select").actions();
        });
      })(django.jQuery);
    </script>
  {% endif %}{% endif %}
{% endblock %}

{% block bodyclass %}change-list{% endblock %}

{% if not is_popup %}
  {% block breadcrumbs %}
    <ul class="breadcrumb">
      <li><a href="{% url 'admin:index' %}">{% trans 'Home' %}</a>
        <span class="divider">&raquo;</span></li>
      <li>
        <a href="{% url 'admin:app_list' app_label=cl.opts.app_label %}">{% firstof cl.opts.app_config.verbose_name app_label|capfirst|escape %}</a>
        <span class="divider">&raquo;</span></li>
      <li class="active">{{ cl.opts.verbose_name_plural|capfirst }}</li>
    </ul>
  {% endblock %}
{% endif %}

{% block coltype %}flex{% endblock %}

{% block content %}

  <div id="content-main">

    <div class="inner-center-column">
      <div class="module{% if cl.has_filters %} filtered{% endif %}" id="changelist">

        <div class="toolbar-content clearfix">
          {% block object-tools %}
            <div class="object-tools">
              {% block object-tools-items %}
                {% if has_add_permission %}
                  <a href="{% url cl.opts|admin_urlname:'add' %}{% if is_popup %}?_popup=1{% endif %}" class="btn btn-success">
                    <i class="icon-plus-sign icon-white"></i>&nbsp;
                    {% blocktrans with cl.opts.verbose_name as name %}Add {{ name }}{% endblocktrans %}
                  </a>
                {% endif %}
              {% endblock %}
            </div>
          {% endblock %}

          {% block search %}
          {% search_form cl %}
          <div class="custom_filter" style="padding: 0px 0px 10px 0px">
            <div>
              <form action="" method="GET" id="customForm">
                {% for s in status %}
                <label for="{{ s.id }}" style="display: inline;">
                  <input type="checkbox" class="order_status" value="{{ s.id }}" name="order_status__in" id="{{ s.id }}"{% if s.get_str_id in request.GET.order_status__in %}checked{% endif %}
                  onclick=""
                  >
                  {{ s.title }}</label>
                {% endfor %}
                <!-- <input type="submit" value="search"> -->
              </form>
            </div>
          </div>
          <div class="clearfix"></div>
          {% endblock %}
        </div>

        {% block date_hierarchy %}
          {% if cl.date_hierarchy %}
            {% date_hierarchy cl %}
          {% endif %}
        {% endblock %}

        {% if cl.formset.errors %}
          <div class="alert alert-error errornote">
            {% if cl.formset.total_error_count == 1 %}{% trans "Please correct the error below." %}{% else %}{% trans "Please correct the errors below." %}{% endif %}
          </div>
          {{ cl.formset.non_form_errors }}
        {% endif %}

        <form id="changelist-form" action="" method="post"
            {% if cl.formset.is_multipart %}
              enctype="multipart/form-data"{% endif %} class="form-inline" novalidate>{% csrf_token %}
          {% if cl.formset %}
            <div>{{ cl.formset.management_form }}</div>
          {% endif %}

          {% block result_list %}
            {% if cl.result_count %}
              {% if action_form and actions_on_top and cl.full_result_count %}
                {% admin_actions %}{% endif %}
              {% result_list_with_context cl %}

              {% if action_form and actions_on_bottom and cl.full_result_count %}
                {% admin_actions %}{% endif %}
            {% else %}
              {% suit_bc_value 1.5 'pop' 1.6 '_popup' as POPUP_VAR %}
              <div class="alert alert-block alert-info">
                {% if cl.full_result_count %}
                  <h4>{% trans 'Nothing found' %}!</h4>
                  <br>
                  <a href="?{% if cl.is_popup %}{{ POPUP_VAR }}=1{% endif %}">{% trans 'Reset search and filters' %}</a>
                {% else %}
                  {% blocktrans with cl.opts.verbose_name_plural|capfirst as name_plural %}{{ name_plural }} are not created yet{% endblocktrans %}.
                  {% if has_add_permission %}<a href="{% url cl.opts|admin_urlname:'add' %}{% if is_popup %}?{{ POPUP_VAR }}=1{% endif %}">
                    {% blocktrans with cl.opts.verbose_name as name %}Add {{ name }}{% endblocktrans %}</a>{% endif %}
                {% endif %}
              </div>
            {% endif %}
          {% endblock %}

          {% block pagination %}
            {% if cl.result_count %}
              {% if action_form and actions_on_bottom and cl.full_result_count %}
                <div class="below-actions">
              {% endif %}
              {% pagination cl %}
              {% if action_form and actions_on_bottom and cl.full_result_count %}
                </div>
              {% endif %}
            {% endif %}
          {% endblock %}
        </form>
      </div>
    </div>
  </div>
  <script>
function removeURLParameter(url, parameter) {
    //prefer to use l.search if you have a location/link object
    var urlparts= url.split('?');   
    if (urlparts.length>=2) {

        var prefix= encodeURIComponent(parameter)+'=';
        var pars= urlparts[1].split(/[&;]/g);

        //reverse iteration as may be destructive
        for (var i= pars.length; i-- > 0;) {    
            //idiom for string.startsWith
            if (pars[i].lastIndexOf(prefix, 0) !== -1) {  
                pars.splice(i, 1);
            }
        }

        url= urlparts[0] + (pars.length > 0 ? '?' + pars.join('&') : "");
        return url;
    } else {
        return url;
    }
}
$('.order_status').click(function(){
  var checked_li = $('.order_status:checked');
  var statusList = [];
  checked_li.each(function(){
    statusList.push($(this).val());
  });
  var url = window.location.search;
  var url_except = removeURLParameter(url, 'order_status__in');
  var i = 0;
  var search = '&order_status__in=';
  for (i = 0; i < statusList.length; i++) {
    if (i == 0) {
      search += statusList[i];
    } else {
      search += '%2C' + statusList[i];
    }
  };
  if (url_except.indexOf('?') > -1){
    url_except += search
  } else {
    url_except += '?' + search
  }
  window.location.href = url_except;
})
  </script>
{% endblock %}
```
{% endraw %}
