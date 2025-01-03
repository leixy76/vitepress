---
outline: deep
---

# اتصال به یک سیستم مدیریت محتوا (CMS) {#connecting-to-a-cms}

## گام‌های کلی {#general-workflow}

اتصال ویت‌پرس به یک سیستم مدیریت محتوا به طور عمده بر اساس [مسیریابی پویا](./routing#dynamic-routes) خواهد بود. حتماً قبل از شروع، با روش کار آن آشنا شوید.

از آنجایی که هر سیستم مدیریت محتوا به طریقی متفاوت کار می‌کند، در اینجا تنها می‌توانیم یک جریان کاری عمومی را ارائه دهیم که شما باید آن را برای حالت خاص خودتان سفارشی کنید.

1. اگر سیستم مدیریت محتوا نیاز به احراز هویت دارد، یک فایل `.env` برای ذخیره توکن‌های API خود ایجاد کنید و آن را بارگذاری کنید:

    ```js
    // posts/[id].paths.js
    import { loadEnv } from 'vitepress'

    const env = loadEnv('', process.cwd())
    ```

2. داده‌های مورد نیاز را از سیستم مدیریت محتوا بازیابی کرده و به شکل داده‌های مسیر مناسب فرمت کنید:

    ```js
    export default {
      async paths() {
        // از کتابخانه مشتری مربوط به سیستم مدیریت محتوا استفاده کنید اگر نیاز دارید
        const data = await (await fetch('https://my-cms-api', {
          headers: {
            // توکن در صورت لزوم
          }
        })).json()

        return data.map(entry => {
          return {
            params: { id: entry.id, /* عنوان، نویسندگان، تاریخ و غیره */ },
            content: entry.content
          }
        })
      }
    }
    ```

3. نمایش محتوا در صفحه:

    ```md
    # {{ $params.title }}

    - نوشته شده توسط {{ $params.author }} در تاریخ {{ $params.date }}

    <!-- @content -->
    ```

## راهنماهای ادغام {#integration-guides}

اگر راهنمایی درباره ادغام ویت‌پرس با یک سیستم مدیریت محتوا خاص نوشته‌اید، لطفاً از لینک "ویرایش این صفحه" زیر استفاده کنید تا آن را ارسال کنید!
