+++
date = '2025-01-24'
draft = false
title = 'Django筆記'
categories = ['筆記']
tags = ['python', 'Django']
+++

# Django 學習筆記

## 1. Django 基礎概念

### 1.1 什麼是 Django

**Web 框架簡介**
Django 是一個高階的 Python Web 框架，鼓勵快速開發和乾淨、實用的設計。它由經驗豐富的開發者構建，解決了 Web 開發的許多麻煩，讓你可以專注於編寫應用程式而無需重新發明輪子。

**Django 的設計哲學**
- **DRY（Don't Repeat Yourself）**：避免重複代碼，提高代碼重用性
- **快速開發**：從概念到完成儘可能快速
- **Clean Design**：鼓勵乾淨、實用的設計

**MVT 架構模式（Model-View-Template）**
- **Model**：資料層，定義資料結構和業務邏輯
- **View**：控制層，處理用戶請求並返回回應
- **Template**：展示層，負責用戶界面的呈現

**Django 的優勢與特色**
- 內建管理介面
- 強大的 ORM 系統
- 完善的安全機制
- 豐富的內建功能
- 活躍的社群支援

### 1.2 環境建置

**Python 環境需求**
```bash
# 檢查 Python 版本（建議 3.8+）
python --version
python3 --version
```

**虛擬環境設置**
```bash
# 使用 venv 建立虛擬環境
python -m venv django_env

# 啟動虛擬環境
# Windows
django_env\Scripts\activate
# macOS/Linux
source django_env/bin/activate

# 停用虛擬環境
deactivate
```

**Django 安裝**
```bash
# 安裝最新版本
pip install Django

# 安裝特定版本
pip install Django==4.2

# 驗證安裝
python -m django --version
```

**開發工具推薦**
- **IDE**：PyCharm、VS Code、Sublime Text
- **VS Code 擴充套件**：Python、Django、SQLite Viewer
- **資料庫工具**：DB Browser for SQLite、pgAdmin

## 2. 專案建立與結構

### 2.1 建立第一個 Django 專案

**建立專案**
```bash
# 建立新專案
django-admin startproject myproject

# 進入專案目錄
cd myproject

# 啟動開發伺服器
python manage.py runserver
```

**專案目錄結構解析**
```
myproject/
    manage.py              # 命令列工具
    myproject/
        __init__.py       # Python 套件標識
        settings.py       # 專案設定
        urls.py          # URL 路由配置
        wsgi.py          # WSGI 部署接口
        asgi.py          # ASGI 部署接口
```

**manage.py 檔案功能**
```bash
# 常用指令
python manage.py runserver      # 啟動開發伺服器
python manage.py startapp       # 建立新應用
python manage.py migrate        # 執行資料庫遷移
python manage.py createsuperuser # 建立超級用戶
python manage.py collectstatic  # 收集靜態檔案
```

**settings.py 基本配置**
```python
# 重要設定項目
DEBUG = True  # 開發模式
ALLOWED_HOSTS = []  # 允許的主機
INSTALLED_APPS = [  # 已安裝的應用
    'django.contrib.admin',
    'django.contrib.auth',
    # ... 其他內建應用
]
DATABASES = {  # 資料庫配置
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

### 2.2 應用程式（App）概念

**建立 App**
```bash
# 建立新應用
python manage.py startapp blog
```

**App 目錄結構**
```
blog/
    __init__.py
    admin.py          # 管理介面配置
    apps.py           # 應用配置
    models.py         # 資料模型
    tests.py          # 測試檔案
    views.py          # 視圖函數
    migrations/       # 資料庫遷移檔案
        __init__.py
```

**App 註冊與配置**
```python
# settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',  # 註冊新應用
]
```

## 3. URL 路由系統

### 3.1 URL 配置

**主要 urls.py**
```python
# myproject/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('blog.urls')),
    path('', include('blog.urls')),  # 根路徑
]
```

**應用 urls.py**
```python
# blog/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('post/<int:id>/', views.post_detail, name='post_detail'),
    path('category/<str:category_name>/', views.category, name='category'),
]
```

**URL 模式（patterns）範例**
```python
urlpatterns = [
    # 基本路徑
    path('', views.index),
    
    # 帶參數的路徑
    path('post/<int:post_id>/', views.post_detail),
    path('category/<slug:slug>/', views.category),
    path('archive/<int:year>/<int:month>/', views.archive),
    
    # 正規表達式路徑
    re_path(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
]
```

### 3.2 進階 URL 配置

**命名空間（namespaces）**
```python
# blog/urls.py
from django.urls import path
from . import views

app_name = 'blog'  # 應用命名空間
urlpatterns = [
    path('', views.index, name='index'),
    path('<int:post_id>/', views.detail, name='detail'),
]

# 在模板中使用
# {% url 'blog:detail' post.id %}
```

**URL 反向解析**
```python
# 在視圖中
from django.urls import reverse
from django.shortcuts import redirect

def my_view(request):
    url = reverse('blog:detail', args=[1])
    return redirect('blog:index')

# 在模板中
{% url 'blog:detail' post.id %}
```

## 4. 視圖（Views）

### 4.1 函數型視圖（Function-based Views）

**基本視圖結構**
```python
# blog/views.py
from django.http import HttpResponse
from django.shortcuts import render, get_object_or_404
from .models import Post

def index(request):
    """首頁視圖"""
    posts = Post.objects.all()
    return render(request, 'blog/index.html', {'posts': posts})

def post_detail(request, post_id):
    """文章詳情視圖"""
    post = get_object_or_404(Post, id=post_id)
    return render(request, 'blog/detail.html', {'post': post})
```

**處理 GET 和 POST 請求**
```python
def contact(request):
    if request.method == 'POST':
        # 處理 POST 資料
        name = request.POST.get('name')
        email = request.POST.get('email')
        message = request.POST.get('message')
        # 處理表單資料...
        return redirect('blog:thank_you')
    else:
        # 顯示表單
        return render(request, 'blog/contact.html')
```

**錯誤處理**
```python
from django.http import Http404

def post_detail(request, post_id):
    try:
        post = Post.objects.get(id=post_id)
    except Post.DoesNotExist:
        raise Http404("文章不存在")
    return render(request, 'blog/detail.html', {'post': post})
```

### 4.2 類別型視圖（Class-based Views）

**基本 CBV**
```python
from django.views import View
from django.views.generic import ListView, DetailView
from .models import Post

class PostListView(ListView):
    model = Post
    template_name = 'blog/post_list.html'
    context_object_name = 'posts'
    paginate_by = 10

class PostDetailView(DetailView):
    model = Post
    template_name = 'blog/post_detail.html'
    context_object_name = 'post'
```

**自訂 CBV**
```python
class PostCreateView(View):
    def get(self, request):
        form = PostForm()
        return render(request, 'blog/create.html', {'form': form})
    
    def post(self, request):
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('blog:index')
        return render(request, 'blog/create.html', {'form': form})
```

## 5. 模板系統（Templates）

### 5.1 模板基礎

**基本模板語法**
```html
<!-- blog/templates/blog/base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}我的部落格{% endblock %}</title>
</head>
<body>
    <header>
        <h1>我的部落格</h1>
        <nav>
            <a href="{% url 'blog:index' %}">首頁</a>
        </nav>
    </header>
    
    <main>
        {% block content %}
        {% endblock %}
    </main>
    
    <footer>
        <p>&copy; 2025 我的部落格</p>
    </footer>
</body>
</html>
```

**模板繼承**
```html
<!-- blog/templates/blog/index.html -->
{% extends 'blog/base.html' %}

{% block title %}首頁 - {{ block.super }}{% endblock %}

{% block content %}
<h2>最新文章</h2>
{% for post in posts %}
    <article>
        <h3><a href="{% url 'blog:detail' post.id %}">{{ post.title }}</a></h3>
        <p>{{ post.content|truncatewords:50 }}</p>
        <small>發佈於 {{ post.created_at|date:"Y-m-d" }}</small>
    </article>
{% empty %}
    <p>還沒有文章。</p>
{% endfor %}
{% endblock %}
```

**常用模板標籤和過濾器**
```html
<!-- 變數輸出 -->
{{ post.title }}
{{ post.content|safe }}

<!-- 條件判斷 -->
{% if user.is_authenticated %}
    <p>歡迎，{{ user.username }}！</p>
{% else %}
    <p><a href="{% url 'login' %}">登入</a></p>
{% endif %}

<!-- 迴圈 -->
{% for post in posts %}
    {{ forloop.counter }}. {{ post.title }}
{% endfor %}

<!-- 常用過濾器 -->
{{ post.content|truncatewords:30 }}
{{ post.created_at|date:"Y-m-d H:i" }}
{{ post.title|upper }}
{{ post.content|linebreaks }}
```

### 5.2 靜態檔案處理

**設定靜態檔案**
```python
# settings.py
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    BASE_DIR / "static",
]
STATIC_ROOT = BASE_DIR / "staticfiles"
```

**在模板中使用靜態檔案**
```html
{% load static %}
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="{% static 'blog/style.css' %}">
</head>
<body>
    <img src="{% static 'blog/images/logo.png' %}" alt="Logo">
</body>
</html>
```

## 6. 模型（Models）與資料庫

### 6.1 ORM 基礎

**基本模型定義**
```python
# blog/models.py
from django.db import models
from django.contrib.auth.models import User

class Category(models.Model):
    name = models.CharField(max_length=100)
    slug = models.SlugField(unique=True)
    description = models.TextField(blank=True)
    
    def __str__(self):
        return self.name
    
    class Meta:
        verbose_name_plural = "categories"

class Post(models.Model):
    STATUS_CHOICES = [
        ('draft', '草稿'),
        ('published', '已發佈'),
    ]
    
    title = models.CharField(max_length=200)
    slug = models.SlugField(unique=True)
    content = models.TextField()
    status = models.CharField(max_length=10, choices=STATUS_CHOICES, default='draft')
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    category = models.ForeignKey(Category, on_delete=models.SET_NULL, null=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.title
    
    class Meta:
        ordering = ['-created_at']
```

**常用欄位類型**
```python
class MyModel(models.Model):
    # 文字欄位
    char_field = models.CharField(max_length=100)
    text_field = models.TextField()
    slug_field = models.SlugField()
    
    # 數字欄位
    integer_field = models.IntegerField()
    float_field = models.FloatField()
    decimal_field = models.DecimalField(max_digits=10, decimal_places=2)
    
    # 日期時間欄位
    date_field = models.DateField()
    datetime_field = models.DateTimeField()
    time_field = models.TimeField()
    
    # 布林欄位
    boolean_field = models.BooleanField()
    
    # 檔案欄位
    image_field = models.ImageField(upload_to='images/')
    file_field = models.FileField(upload_to='files/')
    
    # 其他欄位
    email_field = models.EmailField()
    url_field = models.URLField()
```

### 6.2 資料庫操作

**遷移操作**
```bash
# 建立遷移檔案
python manage.py makemigrations

# 查看遷移 SQL
python manage.py sqlmigrate blog 0001

# 執行遷移
python manage.py migrate

# 查看遷移狀態
python manage.py showmigrations
```

**QuerySet API**
```python
# 查詢所有記錄
Post.objects.all()

# 條件查詢
Post.objects.filter(status='published')
Post.objects.filter(title__contains='Django')
Post.objects.filter(created_at__year=2025)

# 排除查詢
Post.objects.exclude(status='draft')

# 排序
Post.objects.order_by('-created_at')
Post.objects.order_by('title', '-created_at')

# 限制結果
Post.objects.all()[:5]  # 前5筆
Post.objects.all()[5:10]  # 第6-10筆

# 查詢單一記錄
Post.objects.get(id=1)
Post.objects.first()
Post.objects.last()

# 聚合查詢
from django.db.models import Count, Avg
Post.objects.count()
Post.objects.aggregate(Avg('id'))
Post.objects.values('category').annotate(Count('id'))
```

**資料庫關係查詢**
```python
# 一對多查詢
post = Post.objects.get(id=1)
category = post.category  # 取得分類
posts = category.post_set.all()  # 取得分類下的所有文章

# 使用 select_related 優化查詢
posts = Post.objects.select_related('category', 'author').all()

# 使用 prefetch_related 優化多對多查詢
posts = Post.objects.prefetch_related('tags').all()
```

## 7. 表單處理（Forms）

### 7.1 Django 表單基礎

**基本表單定義**
```python
# blog/forms.py
from django import forms
from .models import Post, Category

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    subject = forms.CharField(max_length=200)
    message = forms.CharField(widget=forms.Textarea)
    
    def clean_email(self):
        email = self.cleaned_data['email']
        if not email.endswith('@example.com'):
            raise forms.ValidationError('請使用公司信箱')
        return email

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'slug', 'content', 'category', 'status']
        widgets = {
            'content': forms.Textarea(attrs={'rows': 10}),
            'slug': forms.TextInput(attrs={'placeholder': 'url-slug'}),
        }
    
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields['category'].queryset = Category.objects.all()
```

**在視圖中處理表單**
```python
def create_post(request):
    if request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('blog:detail', post.id)
    else:
        form = PostForm()
    return render(request, 'blog/create.html', {'form': form})

def edit_post(request, post_id):
    post = get_object_or_404(Post, id=post_id)
    if request.method == 'POST':
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            form.save()
            return redirect('blog:detail', post.id)
    else:
        form = PostForm(instance=post)
    return render(request, 'blog/edit.html', {'form': form, 'post': post})
```

**表單模板**
```html
<!-- blog/templates/blog/create.html -->
{% extends 'blog/base.html' %}

{% block content %}
<h2>建立新文章</h2>
<form method="post">
    {% csrf_token %}
    
    {% if form.errors %}
        <div class="alert alert-danger">
            {{ form.errors }}
        </div>
    {% endif %}
    
    <div class="form-group">
        {{ form.title.label_tag }}
        {{ form.title }}
    </div>
    
    <div class="form-group">
        {{ form.slug.label_tag }}
        {{ form.slug }}
    </div>
    
    <div class="form-group">
        {{ form.content.label_tag }}
        {{ form.content }}
    </div>
    
    <div class="form-group">
        {{ form.category.label_tag }}
        {{ form.category }}
    </div>
    
    <div class="form-group">
        {{ form.status.label_tag }}
        {{ form.status }}
    </div>
    
    <button type="submit" class="btn btn-primary">儲存</button>
</form>
{% endblock %}
```

## 8. 用戶認證與權限

### 8.1 用戶認證系統

**基本認證視圖**
```python
# blog/views.py
from django.contrib.auth import authenticate, login, logout
from django.contrib.auth.decorators import login_required
from django.contrib.auth.forms import UserCreationForm

def user_login(request):
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            return redirect('blog:index')
        else:
            messages.error(request, '帳號或密碼錯誤')
    return render(request, 'registration/login.html')

def user_logout(request):
    logout(request)
    return redirect('blog:index')

def register(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            login(request, user)
            return redirect('blog:index')
    else:
        form = UserCreationForm()
    return render(request, 'registration/register.html', {'form': form})

@login_required
def profile(request):
    return render(request, 'blog/profile.html')
```

**登入模板**
```html
<!-- templates/registration/login.html -->
{% extends 'blog/base.html' %}

{% block content %}
<h2>登入</h2>
<form method="post">
    {% csrf_token %}
    <div class="form-group">
        <label for="username">用戶名：</label>
        <input type="text" name="username" required>
    </div>
    <div class="form-group">
        <label for="password">密碼：</label>
        <input type="password" name="password" required>
    </div>
    <button type="submit">登入</button>
</form>
<p><a href="{% url 'register' %}">還沒有帳號？註冊</a></p>
{% endblock %}
```

### 8.2 權限與授權

**權限裝飾器**
```python
from django.contrib.auth.decorators import login_required, permission_required
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin

@login_required
def create_post(request):
    # 只有登入用戶才能建立文章
    pass

@permission_required('blog.add_post')
def admin_create_post(request):
    # 只有有權限的用戶才能建立文章
    pass

class PostCreateView(LoginRequiredMixin, CreateView):
    model = Post
    form_class = PostForm
    login_url = '/login/'
```

**在模板中檢查權限**
```html
{% if user.is_authenticated %}
    <p>歡迎，{{ user.username }}！</p>
    {% if perms.blog.add_post %}
        <a href="{% url 'blog:create' %}">建立文章</a>
    {% endif %}
{% else %}
    <a href="{% url 'login' %}">登入</a>
{% endif %}
```

## 9. 管理介面（Admin）

### 9.1 Django Admin 基礎

**註冊模型到 Admin**
```python
# blog/admin.py
from django.contrib import admin
from .models import Post, Category

@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    list_display = ['name', 'slug']
    prepopulated_fields = {'slug': ('name',)}

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ['title', 'author', 'category', 'status', 'created_at']
    list_filter = ['status', 'category', 'created_at']
    search_fields = ['title', 'content']
    prepopulated_fields = {'slug': ('title',)}
    raw_id_fields = ['author']
    date_hierarchy = 'created_at'
    ordering = ['-created_at']
```

**建立超級用戶**
```bash
python manage.py createsuperuser
```

### 9.2 Admin 進階客製化

**自訂 Admin 動作**
```python
@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    actions = ['make_published', 'make_draft']
    
    def make_published(self, request, queryset):
        updated = queryset.update(status='published')
        self.message_user(request, f'{updated} 篇文章已發佈')
    make_published.short_description = '發佈選中的文章'
    
    def make_draft(self, request, queryset):
        updated = queryset.update(status='draft')
        self.message_user(request, f'{updated} 篇文章已設為草稿')
    make_draft.short_description = '設為草稿'
```

## 10. 靜態檔案與媒體檔案

### 10.1 靜態檔案管理

**設定檔配置**
```python
# settings.py
import os

# 靜態檔案設定
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    BASE_DIR / "static",
]
STATIC_ROOT = BASE_DIR / "staticfiles"

# 媒體檔案設定
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / "media"
```

**URL 配置**
```python
# myproject/urls.py
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # ... 其他 URL 模式
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

**收集靜態檔案**
```bash
# 在生產環境中收集所有靜態檔案
python manage.py collectstatic
```

## 11. 測試

### 11.1 Django 測試框架

**模型測試**
```python
# blog/tests.py
from django.test import TestCase
from django.contrib.auth.models import User
from .models import Post, Category

class PostModelTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(
            username='testuser',
            password='testpass123'
        )
        self.category = Category.objects.create(
            name='測試分類',
            slug='test-category'
        )
    
    def test_post_creation(self):
        post = Post.objects.create(
            title='測試文章',
            slug='test-post',
            content='這是測試內容',
            author=self.user,
            category=self.category
        )
        self.assertEqual(post.title, '測試文章')
        self.assertEqual(str(post), '測試文章')
    
    def test_post_slug_unique(self):
        Post.objects.create(
            title='測試文章1',
            slug='test-post',
            content='內容1',
            author=self.user
        )
        # 建立相同 slug 的文章應該失敗
        with self.assertRaises(Exception):
            Post.objects.create(
                title='測試文章2',
                slug='test-post',
                content='內容2',
                author=self.user
            )
```

**視圖測試**
```python
class PostViewTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(
            username='testuser',
            password='testpass123'
        )
        self.post = Post.objects.create(
            title='測試文章',
            slug='test-post',
            content='測試內容',
            author=self.user,
            status='published'
        )
    
    def test_post_list_view(self):
        response = self.client.get('/')
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, '測試文章')
    
    def test_post_detail_view(self):
        response = self.client.get(f'/post/{self.post.id}/')
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, self.post.title)
    
    def test_post_create_requires_login(self):
        response = self.client.get('/create/')
        self.assertEqual(response.status_code, 302)  # 重定向到登入頁面
        
        # 登入後應該可以存取
        self.client.login(username='testuser', password='testpass123')
        response = self.client.get('/create/')
        self.assertEqual(response.status_code, 200)
```

**執行測試**
```bash
# 執行所有測試
python manage.py test

# 執行特定應用的測試
python manage.py test blog

# 執行特定測試類別
python manage.py test blog.tests.PostModelTest

# 查看測試覆蓋率（需要安裝 coverage）
pip install coverage
coverage run --source='.' manage.py test
coverage report
coverage html
```

## 12. 部署與效能優化

### 12.1 部署準備

**生產環境設定**
```python
# settings/production.py
from .base import *

DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']

# 安全設定
SECURE_BROWSER_XSS_FILTER = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True

# 資料庫設定（PostgreSQL）
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('DB_NAME'),
        'USER': os.environ.get('DB_USER'),
        'PASSWORD': os.environ.get('DB_PASSWORD'),
        'HOST': os.environ.get('DB_HOST', 'localhost'),
        'PORT': os.environ.get('DB_PORT', '5432'),
    }
}

# 快取設定（Redis）
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
        'OPTIONS': {
            'CLIENT_CLASS': 'django_redis.client.DefaultClient',
        }
    }
}

# 郵件設定
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_USE_TLS = True
EMAIL_PORT = 587
EMAIL_HOST_USER = os.environ.get('EMAIL_USER')
EMAIL_HOST_PASSWORD = os.environ.get('EMAIL_PASSWORD')
```

**環境變數管理**
```python
# .env 檔案
DEBUG=False
SECRET_KEY=your-secret-key-here
DB_NAME=myproject_db
DB_USER=myproject_user
DB_PASSWORD=secure_password
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-app-password

# 在 settings.py 中讀取
import os
from pathlib import Path
import environ

env = environ.Env()
environ.Env.read_env()

SECRET_KEY = env('SECRET_KEY')
DEBUG = env.bool('DEBUG', default=False)
```

**requirements.txt**
```text
Django==4.2.7
psycopg2-binary==2.9.7
django-redis==5.3.0
gunicorn==21.2.0
whitenoise==6.5.0
python-environ==0.11.2
Pillow==10.0.1
django-crispy-forms==2.0
```

### 12.2 效能優化

**資料庫查詢優化**
```python
# 使用 select_related 減少查詢次數
posts = Post.objects.select_related('author', 'category').all()

# 使用 prefetch_related 優化多對多關係
posts = Post.objects.prefetch_related('tags').all()

# 使用 only() 只選擇需要的欄位
posts = Post.objects.only('title', 'slug', 'created_at').all()

# 使用 defer() 延遲載入大欄位
posts = Post.objects.defer('content').all()

# 使用原始 SQL 查詢
from django.db import connection

def get_post_statistics():
    with connection.cursor() as cursor:
        cursor.execute("""
            SELECT category_id, COUNT(*) as post_count
            FROM blog_post
            WHERE status = 'published'
            GROUP BY category_id
        """)
        return cursor.fetchall()
```

**快取策略**
```python
# 視圖快取
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)  # 快取 15 分鐘
def post_list(request):
    posts = Post.objects.filter(status='published')
    return render(request, 'blog/list.html', {'posts': posts})

# 模板片段快取
{% load cache %}
{% cache 500 sidebar %}
    <!-- 側邊欄內容 -->
    <div class="sidebar">
        {% for category in categories %}
            <a href="{{ category.get_absolute_url }}">{{ category.name }}</a>
        {% endfor %}
    </div>
{% endcache %}

# 低階快取 API
from django.core.cache import cache

def get_popular_posts():
    popular_posts = cache.get('popular_posts')
    if popular_posts is None:
        popular_posts = Post.objects.filter(
            status='published'
        ).order_by('-views')[:5]
        cache.set('popular_posts', popular_posts, 60 * 30)  # 快取 30 分鐘
    return popular_posts
```

**靜態檔案優化**
```python
# settings.py
# 使用 WhiteNoise 處理靜態檔案
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',  # 添加這行
    # ... 其他中介軟體
]

# 靜態檔案壓縮
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

# 媒體檔案使用 CDN
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
AWS_ACCESS_KEY_ID = 'your-access-key'
AWS_SECRET_ACCESS_KEY = 'your-secret-key'
AWS_STORAGE_BUCKET_NAME = 'your-bucket-name'
```

### 12.3 部署平台

**使用 Gunicorn 部署**
```bash
# 安裝 Gunicorn
pip install gunicorn

# 啟動應用
gunicorn myproject.wsgi:application --bind 0.0.0.0:8000

# 使用配置檔案
# gunicorn.conf.py
bind = "0.0.0.0:8000"
workers = 4
worker_class = "sync"
worker_connections = 1000
max_requests = 1000
max_requests_jitter = 100
timeout = 30
keepalive = 2
```

**Nginx 配置**
```nginx
# /etc/nginx/sites-available/myproject
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    
    location /static/ {
        alias /path/to/your/project/staticfiles/;
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    location /media/ {
        alias /path/to/your/project/media/;
        expires 1y;
        add_header Cache-Control "public";
    }
    
    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Docker 部署**
```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

RUN python manage.py collectstatic --noinput

EXPOSE 8000

CMD ["gunicorn", "myproject.wsgi:application", "--bind", "0.0.0.0:8000"]
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DEBUG=False
      - DB_HOST=db
    depends_on:
      - db
      - redis
    volumes:
      - static_volume:/app/staticfiles
      - media_volume:/app/media

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: myproject_db
      POSTGRES_USER: myproject_user
      POSTGRES_PASSWORD: secure_password
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
      - static_volume:/static
      - media_volume:/media
    depends_on:
      - web

volumes:
  postgres_data:
  static_volume:
  media_volume:
```

## 13. Django REST Framework（API 開發）

### 13.1 API 開發基礎

**安裝和設定 DRF**
```bash
pip install djangorestframework
```

```python
# settings.py
INSTALLED_APPS = [
    # ... 其他應用
    'rest_framework',
    'blog',
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 20
}
```

**序列化器（Serializers）**
```python
# blog/serializers.py
from rest_framework import serializers
from .models import Post, Category

class CategorySerializer(serializers.ModelSerializer):
    class Meta:
        model = Category
        fields = ['id', 'name', 'slug', 'description']

class PostSerializer(serializers.ModelSerializer):
    author = serializers.StringRelatedField(read_only=True)
    category = CategorySerializer(read_only=True)
    category_id = serializers.IntegerField(write_only=True)
    
    class Meta:
        model = Post
        fields = [
            'id', 'title', 'slug', 'content', 'status',
            'author', 'category', 'category_id',
            'created_at', 'updated_at'
        ]
        read_only_fields = ['created_at', 'updated_at']
    
    def validate_title(self, value):
        if len(value) < 5:
            raise serializers.ValidationError('標題至少需要 5 個字元')
        return value

class PostListSerializer(serializers.ModelSerializer):
    """用於列表顯示的簡化序列化器"""
    author = serializers.StringRelatedField()
    category = serializers.StringRelatedField()
    
    class Meta:
        model = Post
        fields = ['id', 'title', 'slug', 'author', 'category', 'created_at']
```

**API 視圖**
```python
# blog/api_views.py
from rest_framework import generics, permissions, status
from rest_framework.decorators import api_view, permission_classes
from rest_framework.response import Response
from .models import Post, Category
from .serializers import PostSerializer, PostListSerializer, CategorySerializer

class PostListCreateAPIView(generics.ListCreateAPIView):
    queryset = Post.objects.filter(status='published')
    serializer_class = PostListSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    
    def get_serializer_class(self):
        if self.request.method == 'POST':
            return PostSerializer
        return PostListSerializer
    
    def perform_create(self, serializer):
        serializer.save(author=self.request.user)

class PostDetailAPIView(generics.RetrieveUpdateDestroyAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    
    def get_permissions(self):
        if self.request.method in ['PUT', 'PATCH', 'DELETE']:
            return [permissions.IsAuthenticated()]
        return [permissions.AllowAny()]

@api_view(['GET'])
@permission_classes([permissions.AllowAny])
def category_list(request):
    categories = Category.objects.all()
    serializer = CategorySerializer(categories, many=True)
    return Response(serializer.data)

@api_view(['GET'])
def post_by_category(request, category_slug):
    try:
        category = Category.objects.get(slug=category_slug)
        posts = Post.objects.filter(category=category, status='published')
        serializer = PostListSerializer(posts, many=True)
        return Response(serializer.data)
    except Category.DoesNotExist:
        return Response(
            {'error': '分類不存在'}, 
            status=status.HTTP_404_NOT_FOUND
        )
```

**API URL 配置**
```python
# blog/urls.py
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from . import views, api_views

# API URLs
api_urlpatterns = [
    path('posts/', api_views.PostListCreateAPIView.as_view(), name='post-list'),
    path('posts/<int:pk>/', api_views.PostDetailAPIView.as_view(), name='post-detail'),
    path('categories/', api_views.category_list, name='category-list'),
    path('categories/<slug:category_slug>/posts/', api_views.post_by_category, name='posts-by-category'),
]

urlpatterns = [
    # 一般視圖
    path('', views.index, name='index'),
    path('post/<int:id>/', views.post_detail, name='post_detail'),
    
    # API 端點
    path('api/', include(api_urlpatterns)),
]
```

### 13.2 進階 API 功能

**ViewSets 和 Routers**
```python
# blog/viewsets.py
from rest_framework import viewsets, filters
from rest_framework.decorators import action
from rest_framework.response import Response
from django_filters.rest_framework import DjangoFilterBackend
from .models import Post
from .serializers import PostSerializer, PostListSerializer

class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    filter_backends = [DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter]
    filterset_fields = ['category', 'status']
    search_fields = ['title', 'content']
    ordering_fields = ['created_at', 'title']
    ordering = ['-created_at']
    
    def get_serializer_class(self):
        if self.action == 'list':
            return PostListSerializer
        return PostSerializer
    
    @action(detail=True, methods=['post'])
    def set_published(self, request, pk=None):
        post = self.get_object()
        post.status = 'published'
        post.save()
        return Response({'status': 'published'})
    
    @action(detail=False)
    def published(self, request):
        published_posts = Post.objects.filter(status='published')
        serializer = self.get_serializer(published_posts, many=True)
        return Response(serializer.data)
```

```python
# blog/urls.py (使用 Router)
from rest_framework.routers import DefaultRouter
from .viewsets import PostViewSet

router = DefaultRouter()
router.register(r'posts', PostViewSet)

urlpatterns = [
    path('api/', include(router.urls)),
]
```

**認證和權限**
```python
# 自訂權限
from rest_framework import permissions

class IsAuthorOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        # 讀取權限給所有人
        if request.method in permissions.SAFE_METHODS:
            return True
        # 寫入權限只給作者
        return obj.author == request.user

# 在視圖中使用
class PostDetailAPIView(generics.RetrieveUpdateDestroyAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly, IsAuthorOrReadOnly]
```

**Token 認證**
```python
# settings.py
INSTALLED_APPS = [
    # ...
    'rest_framework.authtoken',
]

# 為用戶建立 Token
from rest_framework.authtoken.models import Token
from django.contrib.auth.models import User

# 為所有用戶建立 Token
for user in User.objects.all():
    Token.objects.get_or_create(user=user)

# API 登入視圖
from rest_framework.authtoken.views import ObtainAuthToken
from rest_framework.authtoken.models import Token
from rest_framework.response import Response

class CustomAuthToken(ObtainAuthToken):
    def post(self, request, *args, **kwargs):
        serializer = self.serializer_class(data=request.data,
                                           context={'request': request})
        serializer.is_valid(raise_exception=True)
        user = serializer.validated_data['user']
        token, created = Token.objects.get_or_create(user=user)
        return Response({
            'token': token.key,
            'user_id': user.pk,
            'email': user.email
        })
```

## 14. 常用第三方套件

### 14.1 開發工具

**Django Debug Toolbar**
```bash
pip install django-debug-toolbar
```

```python
# settings.py
INSTALLED_APPS = [
    # ...
    'debug_toolbar',
]

MIDDLEWARE = [
    # ...
    'debug_toolbar.middleware.DebugToolbarMiddleware',
]

INTERNAL_IPS = [
    '127.0.0.1',
]

# urls.py
import debug_toolbar
from django.conf import settings

if settings.DEBUG:
    urlpatterns = [
        path('__debug__/', include(debug_toolbar.urls)),
    ] + urlpatterns
```

**Django Extensions**
```bash
pip install django-extensions
```

```python
# settings.py
INSTALLED_APPS = [
    # ...
    'django_extensions',
]
```

```bash
# 有用的指令
python manage.py shell_plus  # 增強的 shell
python manage.py show_urls   # 顯示所有 URL
python manage.py graph_models -a -o models.png  # 生成模型關係圖
```

**Django Crispy Forms**
```bash
pip install django-crispy-forms crispy-bootstrap4
```

```python
# settings.py
INSTALLED_APPS = [
    # ...
    'crispy_forms',
    'crispy_bootstrap4',
]

CRISPY_TEMPLATE_PACK = 'bootstrap4'
```

```html
<!-- 在模板中使用 -->
{% load crispy_forms_tags %}

<form method="post">
    {% csrf_token %}
    {{ form|crispy }}
    <button type="submit" class="btn btn-primary">提交</button>
</form>
```

### 14.2 功能擴展

**Celery（非同步任務）**
```bash
pip install celery redis
```

```python
# myproject/celery.py
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')

app = Celery('myproject')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()

# settings.py
CELERY_BROKER_URL = 'redis://localhost:6379'
CELERY_RESULT_BACKEND = 'redis://localhost:6379'
CELERY_ACCEPT_CONTENT = ['application/json']
CELERY_TASK_SERIALIZER = 'json'
CELERY_RESULT_SERIALIZER = 'json'

# blog/tasks.py
from celery import shared_task
from django.core.mail import send_mail

@shared_task
def send_notification_email(post_id):
    from .models import Post
    post = Post.objects.get(id=post_id)
    send_mail(
        '新文章發佈',
        f'新文章「{post.title}」已發佈',
        'from@example.com',
        ['to@example.com'],
        fail_silently=False,
    )

# 在視圖中使用
def publish_post(request, post_id):
    post = get_object_or_404(Post, id=post_id)
    post.status = 'published'
    post.save()
    
    # 非同步發送通知郵件
    send_notification_email.delay(post_id)
    
    return redirect('blog:detail', post_id)
```

**Django Filters**
```bash
pip install django-filter
```

```python
# blog/filters.py
import django_filters
from .models import Post

class PostFilter(django_filters.FilterSet):
    title = django_filters.CharFilter(lookup_expr='icontains')
    created_at = django_filters.DateFromToRangeFilter()
    
    class Meta:
        model = Post
        fields = ['category', 'status']

# blog/views.py
from django_filters.views import FilterView
from .filters import PostFilter

class PostFilterView(FilterView):
    model = Post
    filterset_class = PostFilter
    template_name = 'blog/post_filter.html'
    paginate_by = 10
```

## 15. 最佳實務與常見問題

### 15.1 程式碼品質

**專案結構最佳實務**
```
myproject/
├── manage.py
├── requirements/
│   ├── base.txt
│   ├── development.txt
│   └── production.txt
├── myproject/
│   ├── __init__.py
│   ├── settings/
│   │   ├── __init__.py
│   │   ├── base.py
│   │   ├── development.py
│   │   └── production.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── apps/
│   ├── blog/
│   ├── users/
│   └── core/
├── static/
├── media/
├── templates/
├── locale/
└── tests/
```

**設定檔分離**
```python
# settings/base.py
from pathlib import Path
import environ

env = environ.Env()

BASE_DIR = Path(__file__).resolve().parent.parent.parent

# 基本設定
DJANGO_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

THIRD_PARTY_APPS = [
    'rest_framework',
    'crispy_forms',
]

LOCAL_APPS = [
    'apps.blog',
    'apps.users',
    'apps.core',
]

INSTALLED_APPS = DJANGO_APPS + THIRD_PARTY_APPS + LOCAL_APPS

# settings/development.py
from .base import *

DEBUG = True
ALLOWED_HOSTS = ['localhost', '127.0.0.1']

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# settings/production.py
from .base import *

DEBUG = False
ALLOWED_HOSTS = env.list('ALLOWED_HOSTS')

DATABASES = {
    'default': env.db()
}
```

**模型最佳實務**
```python
from django.db import models
from django.urls import reverse
from django.utils.text import slugify

class TimestampedModel(models.Model):
    """抽象基礎模型，提供時間戳欄位"""
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        abstract = True

class Post(TimestampedModel):
    title = models.CharField(max_length=200)
    slug = models.SlugField(unique=True, blank=True)
    content = models.TextField()
    
    def save(self, *args, **kwargs):
        if not self.slug:
            self.slug = slugify(self.title)
        super().save(*args, **kwargs)
    
    def get_absolute_url(self):
        return reverse('blog:detail', kwargs={'slug': self.slug})
    
    def __str__(self):
        return self.title
    
    class Meta:
        ordering = ['-created_at']
        verbose_name = '文章'
        verbose_name_plural = '文章'
```

### 15.2 常見問題解決

**效能問題診斷**
```python
# 使用 Django Debug Toolbar 查看 SQL 查詢
# 安裝 django-querycount 監控查詢數量
pip install django-querycount

# settings.py
MIDDLEWARE = [
    'querycount.middleware.QueryCountDebugMiddleware',
    # ... 其他中介軟體
]

# 查詢優化範例
# 錯誤做法：N+1 查詢問題
posts = Post.objects.all()
for post in posts:
    print(post.author.username)  # 每次都會查詢資料庫

# 正確做法：使用 select_related
posts = Post.objects.select_related('author').all()
for post in posts:
    print(post.author.username)  # 只查詢一次
```

**記憶體使用優化**
```python
# 處理大量資料時使用 iterator()
for post in Post.objects.all().iterator():
    # 處理每個 post
    process_post(post)

# 使用 bulk_create 批次建立
posts_to_create = [
    Post(title=f'文章 {i}', content=f'內容 {i}')
    for i in range(1000)
]
Post.objects.bulk_create(posts_to_create, batch_size=100)

# 使用 bulk_update 批次更新
posts = Post.objects.all()
for post in posts:
    post.title = post.title.upper()
Post.objects.bulk_update(posts, ['title'], batch_size=100)
```

**除錯技巧**
```python
# 使用 Django shell 進行除錯
python manage.py shell_plus

# 在程式碼中設置中斷點
import pdb; pdb.set_trace()

# 使用 logging 記錄錯誤
import logging

logger = logging.getLogger(__name__)

def my_view(request):
    try:
        # 你的程式碼
        pass
    except Exception as e:
        logger.error(f'錯誤發生在 my_view: {e}')
        raise

# settings.py 中設定 logging
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'DEBUG',
            'class': 'logging.FileHandler',
            'filename': 'debug.log',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'level': 'DEBUG',
            'propagate': True,
        },
    },
}
```

## 附錄

### A. 指令速查表

```bash
# 專案管理
django-admin startproject myproject
python manage.py startapp myapp
python manage.py runserver
python manage.py runserver 0.0.0.0:8000

# 資料庫相關
python manage.py makemigrations
python manage.py migrate
python manage.py showmigrations
python manage.py sqlmigrate app_name 0001

# 用戶管理
python manage.py createsuperuser
python manage.py changepassword username

# 靜態檔案
python manage.py collectstatic
python manage.py findstatic filename

# 測試
python manage.py test
python manage.py test app_name
python manage.py test app_name.tests.TestClass.test_method

# Shell 和除錯
python manage.py shell
python manage.py shell_plus  # 需要 django-extensions
python manage.py dbshell

# 其他有用指令
python manage.py check
python manage.py inspectdb  # 從現有資料庫生成模型
python manage.py dumpdata app_name > data.json
python manage.py loaddata data.json
```

### B. 設定檔案範例

**完整的 settings.py 範例**
```python
from pathlib import Path
import environ
import os

# 環境變數
env = environ.Env(
    DEBUG=(bool, False)
)
environ.Env.read_env()

# 基本設定
BASE_DIR = Path(__file__).resolve().parent.parent
SECRET_KEY = env('SECRET_KEY')
DEBUG = env('DEBUG')
ALLOWED_HOSTS = env.list('ALLOWED_HOSTS', default=[])

# 應用設定
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    # 第三方應用
    'rest_framework',
    'crispy_forms',
    'debug_toolbar',
    
    # 本地應用
    'blog',
    'users',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'debug_toolbar.middleware.DebugToolbarMiddleware',
]

ROOT_URLCONF = 'myproject.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'myproject.wsgi.application'

# 資料庫設定
DATABASES = {
    'default': env.db()
}

# 密碼驗證
AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

# 國際化
LANGUAGE_CODE = 'zh-hant'
TIME_ZONE = 'Asia/Taipei'
USE_I18N = True
USE_TZ = True

# 靜態檔案設定
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
STATIC_ROOT = BASE_DIR / 'staticfiles'

# 媒體檔案設定
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

# 預設主鍵類型
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

# 快取設定
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': env('REDIS_URL', default='redis://127.0.0.1:6379/1'),
        'OPTIONS': {
            'CLIENT_CLASS': 'django_redis.client.DefaultClient',
        }
    }
}

# 郵件設定
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = env('EMAIL_HOST', default='localhost')
EMAIL_PORT = env.int('EMAIL_PORT', default=587)
EMAIL_USE_TLS = env.bool('EMAIL_USE_TLS', default=True)
EMAIL_HOST_USER = env('EMAIL_HOST_USER', default='')
EMAIL_HOST_PASSWORD = env('EMAIL_HOST_PASSWORD', default='')

# REST Framework 設定
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    ],
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 20,
    'DEFAULT_FILTER_BACKENDS': [
        'django_filters.rest_framework.DjangoFilterBackend',
        'rest_framework.filters.SearchFilter',
        'rest_framework.filters.OrderingFilter',
    ],
}

# Crispy Forms 設定
CRISPY_TEMPLATE_PACK = 'bootstrap4'

# Debug Toolbar 設定
INTERNAL_IPS = [
    '127.0.0.1',
]

# 安全設定（生產環境）
if not DEBUG:
    SECURE_BROWSER_XSS_FILTER = True
    SECURE_CONTENT_TYPE_NOSNIFF = True
    SECURE_HSTS_SECONDS = 31536000
    SECURE_HSTS_INCLUDE_SUBDOMAINS = True
    SECURE_HSTS_PRELOAD = True
    SECURE_SSL_REDIRECT = True
    SESSION_COOKIE_SECURE = True
    CSRF_COOKIE_SECURE = True
    X_FRAME_OPTIONS = 'DENY'
```

### C. 實用資源連結

**官方文件**
- Django 官方文件：https://docs.djangoproject.com/
- Django REST Framework：https://www.django-rest-framework.org/
- Django 套件索引：https://djangopackages.org/

**學習資源**
- Django Girls 教學：https://tutorial.djangogirls.org/
- Mozilla Django 教學：https://developer.mozilla.org/zh-TW/docs/Learn/Server-side/Django
- Real Python Django 教學：https://realpython.com/tutorials/django/

**開發工具**
- Django Debug Toolbar：https://django-debug-toolbar.readthedocs.io/
- Django Extensions：https://django-extensions.readthedocs.io/
- Django Crispy Forms：https://django-crispy-forms.readthedocs.io/

**部署相關**
- Gunicorn：https://gunicorn.org/
- WhiteNoise：http://whitenoise.evans.io/
- Docker：https://docs.docker.com/