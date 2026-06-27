# ক্লাস নোট (Class Note) — মডিউল ০৪

**কোর্সের নাম:** অ্যাডভান্সড শপিফাই থিম ডেভেলপমেন্ট (Advanced Shopify Theme Development)

**মডিউল ০৪:** ওএস ২.০ জেসন টেমপ্লেট (JSON Templates) এবং কাস্টম সেকশন ও স্কিমা (Schema) ডেভেলপমেন্ট

**ইনস্ট্রাক্টর:** Md. Nayemur Rahman

**প্রতিষ্ঠান:** ProjuktiPlus

---

### 📌 ১. ভূমিকা: ওএস ২.০-তে জেসন টেমপ্লেট (JSON Templates) কেন বাধ্যতামূলক?

শপিফাই অনলাইন স্টোর ২.০-এর আগে সব টেমপ্লেট হতো `.liquid` ফরম্যাটে, যার ফলে মার্চেন্টরা কোড না জেনে পেজের সেকশন বা ব্লকের অর্ডার পরিবর্তন করতে পারতেন না। কিন্তু আধুনিক OS 2.0 আর্কিটেকচারে সব মেইন টেমপ্লেট ফাইল (যেমন: Index, Product, Collection) তৈরি হয় **`.json`** ফরম্যাটে।

**জেসন টেমপ্লেটের মূল কাজ:**

* এটি পেজের ভেতরের ডেটা বা ডিজাইন ধারণ করে না, বরং কোন কোন **Sections** কোন অর্ডারে রেন্ডার হবে তার একটা ম্যাপ বা আর্কিটেকচার তৈরি করে।
* এর ফলে মার্চেন্টরা শপিফাই কাস্টমাইজার (Theme Customizer) থেকে ড্র্যাগ-অ্যান্ড-ড্রপ করে যেকোনো পেজে আনলিমিটেড সেকশন যুক্ত ও মুভ করতে পারেন।

---

### 📌 ২. প্র্যাক্টিকাল ল্যাব ১: ইনডেক্স টেমপ্লেট (`index.json`) তৈরি

ভিডিওতে আপনার প্রজেক্টের `templates/` ফোল্ডারে যান এবং একটি নতুন ফাইল তৈরি করুন: `templates/index.json`। এআই অ্যাসিস্ট্যান্ট (Cascade)-কে বলুন এর জন্য একটি মিনিমাল ভ্যালিড স্ট্রাকচার তৈরি করতে:

```json
{
  "name": "Home page",
  "sections": {
    "featured-banner": {
      "type": "hero-banner",
      "settings": {
        "heading": "Welcome to ProjuktiPlus Store!",
        "subheading": "Discover our premium products developed with AI-native workflows."
      }
    }
  },
  "order": [
    "featured-banner"
  ]
}

```

> **লজিক চেক (শিক্ষার্থীদের বোঝানোর জন্য):** এখানে `"type": "hero-banner"` দিয়ে শপিফাইকে বলা হচ্ছে যে এই ইনডেক্স পেজটি আমাদের `sections/` ফোল্ডারে থাকা `hero-banner.liquid` নামের সেকশন ফাইলটিকে রেন্ডার করবে।

---

### 📌 ৩. কাস্টম সেকশন এবং স্কিমা (Schema) ডেভেলপমেন্ট

শপিফাই থিমের প্রাণ হলো এর কাস্টম সেকশন। একটি সেকশন ফাইলে প্রধানত ৩টি অংশ থাকে: **HTML/Liquid কোড, CSS/JS স্টাইলিং এবং `{% schema %}` ট্যাগ**। স্কিমা ট্যাগের মাধ্যমেই ব্যাকএন্ডের কাস্টমাইজার প্যানেল তৈরি হয়।

চলুন আমাদের `sections/` ফোল্ডারে একটি নতুন ফাইল তৈরি করি: `sections/hero-banner.liquid`।

#### **লাইভ কোড ডেমো (Sections & Schema):**

```liquid
<div class="hero-banner" style="background-color: {{ section.settings.bg_color }}; padding: 60px 20px; text-align: center;">
  <div class="container">
    <h1 style="font-size: 2.5rem; margin-bottom: 10px;">{{ section.settings.heading }}</h1>
    <p style="font-size: 1.2rem; color: #555;">{{ section.settings.subheading }}</p>
    
    {% if section.settings.button_label != blank %}
      <a href="{{ section.settings.button_link }}" class="btn" style="display: inline-block; background: #000; color: #fff; padding: 10px 20px; text-decoration: none; margin-top: 15px;">
        {{ section.settings.button_label }}
      </a>
    {% endif %}
  </div>
</div>

{% schema %}
{
  "name": "Hero Banner",
  "tag": "section",
  "class": "section-hero-banner",
  "settings": [
    {
      "type": "text",
      "id": "heading",
      "label": "Heading",
      "default": "Welcome to ProjuktiPlus"
    },
    {
      "type": "textarea",
      "id": "subheading",
      "label": "Subheading",
      "default": "Crafting premium user experiences."
    },
    {
      "type": "url",
      "id": "button_link",
      "label": "Button Link"
    },
    {
      "type": "text",
      "id": "button_label",
      "label": "Button Label",
      "default": "Shop Now"
    },
    {
      "type": "color",
      "id": "bg_color",
      "label": "Background Color",
      "default": "#f4f4f4"
    }
  ],
  "presets": [
    {
      "name": "Hero Banner"
    }
  ]
}
{% endschema %}

```

---

### 📌 ৪. এআই-নেটিভ ওয়ার্কফ্লো: স্কিমা টাইপস এবং ইনপুট সেটিংস বোঝা

ভিডিও স্ক্রিনে এই কোডটি দেখানোর সময় এআই অ্যাসিস্ট্যান্টকে ব্যবহার করার ট্রিকস এবং স্কিমার মূল প্রপসগুলো শিক্ষার্থীদের ইন-ডিটেইলস ভেঙে বুঝিয়ে বলুন:

1. **`id` এবং `section.settings` এর সম্পর্ক:** স্কিমার ভেতরে আমরা যে ইউনিক `id` (যেমন: `heading`) দেবো, লিকুইড কোডে ডেটা প্রিন্ট করার সময় ঠিক সেটাই `{{ section.settings.heading }}` হিসেবে কল করতে হবে।
2. **`presets` এর গুরুত্ব:** যদি কোনো সেকশনে `presets` অংশটি না থাকে, তবে মার্চেন্ট শপিফাই কাস্টমাইজার থেকে **"Add Section"** বাটনে ক্লিক করে ওই সেকশনটি খুঁজে পাবেন না। থিম স্টোর স্ট্যান্ডার্ডের জন্য এটি আবশ্যিক।
3. **এআই প্রম্পট হ্যাক (Windsurf Cascade):** শিক্ষার্থীদের বলুন—সব ইনপুট টাইপ মুখস্থ করার কোনো প্রয়োজন নেই। উইন্ডসার্ফের এআইকে সরাসরি প্রম্পট দিয়ে জটিল স্কিমা তৈরি করা যায়।
> **AI Prompt Example:** "আমার এই হিরো ব্যানার সেকশনে একটি ইমেজ পিকার (`image_picker`) এবং ফন্ট সাইজ কন্ট্রোল করার জন্য একটি রেঞ্জ স্লাইডার (`range`) সেটিংস যুক্ত করে স্কিমা আপডেট করে দাও।"



---

### 📌 ৫. টার্মিনাল টেস্ট: `shopify theme dev` দিয়ে লাইভ প্রিভিউ

এখন আমাদের তৈরি করা জেসন টেমপ্লেট এবং কাস্টম সেকশনটি শপিফাই স্টোরে কেমন দেখাচ্ছে তা লাইভ টেস্ট করার পালা।

1. উইন্ডসার্ফের টার্মিনাল ওপেন করুন।
2. নিচের কমান্ডটি রান করুন:
```bash
shopify theme dev --store=projuktiplus-theme-test

```


3. কমান্ডটি সফলভাবে রান হলে টার্মিনালে একটি **Development Server Link** (যেমন: `[http://127.0.0.1:9292](http://127.0.0.1:9292)`) এবং একটি **Customize Link** জেনারেট হবে।
4. কাস্টমাইজ লিংকে ক্লিক করে ব্রাউজারে প্রবেশ করুন এবং শিক্ষার্থীদের লাইভ দেখান কীভাবে আপনার তৈরি করা "Hero Banner"-এর টেক্সট এবং কালার ব্যাকএন্ড থেকে চেঞ্জ করা যাচ্ছে এবং কোডে কোনো রিফ্রেশ ছাড়াই তা ইনস্ট্যান্ট আপডেট হচ্ছে।

---

### 🎬 ভিডিও রেকর্ডিংয়ের জন্য এন্ডিং স্পিচ (Outro)

*"তাহলে আজকের মডিউলে আমরা শপিফাই অনলাইন স্টোর ২.০-এর সবচেয়ে পাওয়ারফুল ফিচার জেসন টেমপ্লেট (`index.json`) এবং কাস্টম সেকশনের স্কিমা আর্কিটেকচার প্র্যাক্টিকালি শেষ করলাম। আমরা দেখলাম কীভাবে ব্যাকএন্ড কাস্টমাইজারের সাথে ফ্রন্টএন্ড লিকুইড কোড কানেক্ট করতে হয়। পরবর্তী মডিউলে আমরা শপিফাইয়ের রি-ইউজেবল ব্লকস (Blocks) এবং কাস্টম ডাইনামিক প্রোডাক্ট পেজ তৈরি করার অ্যাডভান্সড টেকনিকগুলো শিখবো। আজকের কোডটি প্র্যাকটিস করে গিটহাবে পুশ করে দিন। দেখা হচ্ছে আগামী ক্লাসে, ধন্যবাদ!"*