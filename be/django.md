---
id: django_guide
name: Django Guide
description: Django 開發最佳實踐
---

# 🐍 Django 指南

## 專案建立

```bash
pip install django
django-admin startproject myproject
cd myproject
python manage.py startapp core
python manage.py runserver
```

---

## 專案結構

```
myproject/
├── myproject/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── core/
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── serializers.py
│   └── templates/
├── static/
├── templates/
└── manage.py
```

---

## Model

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    is_published = models.BooleanField(default=False)

    class Meta:
        ordering = ['-created_at']

    def __str__(self):
        return self.title
```

---

## View

```python
from django.shortcuts import render, get_object_or_404
from django.views.generic import ListView, DetailView
from .models import Post

# 函式視圖
def post_list(request):
    posts = Post.objects.filter(is_published=True)
    return render(request, 'posts/list.html', {'posts': posts})

# 類別視圖
class PostListView(ListView):
    model = Post
    template_name = 'posts/list.html'
    context_object_name = 'posts'
    queryset = Post.objects.filter(is_published=True)
```

---

## URL

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.PostListView.as_view(), name='post_list'),
    path('<int:pk>/', views.PostDetailView.as_view(), name='post_detail'),
]
```

---

## Template

```html
{% extends 'base.html' %}

{% block content %}
<h1>文章列表</h1>
{% for post in posts %}
  <article>
    <h2>{{ post.title }}</h2>
    <p>{{ post.content|truncatewords:50 }}</p>
    <a href="{% url 'post_detail' post.pk %}">閱讀更多</a>
  </article>
{% empty %}
  <p>沒有文章</p>
{% endfor %}
{% endblock %}
```

---

## Django REST Framework

```python
from rest_framework import viewsets
from .models import Post
from .serializers import PostSerializer

class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
```

---

## 最佳實踐

1. **分離應用程式**
2. **使用 Class-Based Views**
3. **環境變數管理設定**
4. **DRF 建立 API**
5. **Celery 處理背景任務**
