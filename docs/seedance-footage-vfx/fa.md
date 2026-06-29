# راهنمای فارسی Seedance Footage VFX

این skill برای نوشتن، بازنویسی و بهبود promptهای Seedance 2.0 در حالت video-to-video است؛ یعنی وقتی کاربر یک ویدیوی واقعی دارد و می‌خواهد همان کلیپ حفظ شود، اما یک تغییر VFX روی آن اعمال شود.

نمونه کاربردها:

- اضافه کردن آتش، موجود، افکت نامرئی شدن یا تغییر عضو بدن روی ویدیوی واقعی.
- جایگزین کردن محیط اطراف سوژه، در حالی که چهره، لباس، حرکت دوربین و عملکرد سوژه حفظ می‌شود.
- ساخت prompt برای crash zoom یا push-in هماهنگ با دیالوگ یا تایم‌کد مشخص.
- تولید start frame یا first frame تغییر‌یافته قبل از خرج کردن credit ویدیو.
- اصلاح promptهای قبلی Seedance از نظر نور، مقیاس، زمان‌بندی، creature، SFX یا runtime.

## تشخیص فرمت

فایل ورودی `higgsfiled-seedance-footage-vfx_.skill` یک package سازگار با Claude بود: در واقع یک ZIP با پوشه `seedance-footage-vfx` و فایل `SKILL.md` داخل آن. خود `SKILL.md` از فرمت Agent Skills استفاده می‌کند و از نظر ساختار برای Claude مناسب است. برای Codex، نسخه جداگانه ساخته شد و `agents/openai.yaml` هم اضافه شد.

## مسیرها در ریپو

```text
skills/codex/seedance-footage-vfx/
  SKILL.md
  agents/openai.yaml
  references/

skills/claude/seedance-footage-vfx/
  SKILL.md
  references/
```

دو reference داخلی هم اضافه شده‌اند تا ارجاع‌های داخل skill کامل باشند:

- `references/dialogue-timing.md`
- `references/first-frame.md`

## نصب برای Codex

از ریشه ریپو:

```powershell
$skillsRoot = Join-Path $env:USERPROFILE ".codex\skills"
New-Item -ItemType Directory -Path $skillsRoot -Force | Out-Null
Copy-Item -Path ".\skills\codex\seedance-footage-vfx" -Destination $skillsRoot -Recurse -Force
```

اگر `CODEX_HOME` تنظیم شده، skill را در مسیر زیر نصب کنید:

```text
$CODEX_HOME/skills/seedance-footage-vfx
```

نمونه استفاده در Codex:

```text
Use $seedance-footage-vfx to write a Seedance 2.0 video-to-video prompt for this source clip. Preserve the person and camera move, but make the right arm turn invisible with realistic refraction.
```

## نصب برای Claude Code

macOS/Linux:

```bash
mkdir -p ~/.claude/skills
cp -R skills/claude/seedance-footage-vfx ~/.claude/skills/
```

Windows PowerShell:

```powershell
$skillsRoot = Join-Path $env:USERPROFILE ".claude\skills"
New-Item -ItemType Directory -Path $skillsRoot -Force | Out-Null
Copy-Item -Path ".\skills\claude\seedance-footage-vfx" -Destination $skillsRoot -Recurse -Force
```

نمونه استفاده در Claude Code:

```text
/seedance-footage-vfx write a prompt for this video: preserve the speaker and source dialogue, add a giant photoreal creature on the tower, then push in on the spoken line.
```

## ساخت ZIP برای Claude.ai

```powershell
New-Item -ItemType Directory -Path ".\dist" -Force | Out-Null
Compress-Archive -Path ".\skills\claude\seedance-footage-vfx" -DestinationPath ".\dist\seedance-footage-vfx-claude.zip" -Force
```

فایل `dist/seedance-footage-vfx-claude.zip` را در بخش Skills در Claude.ai آپلود و فعال کنید.

## نکته‌های استفاده

- اگر ویدیوی منبع وجود ندارد یا توضیح داده نشده، skill باید اول درباره footage سؤال بپرسد.
- اگر دیالوگ باید حفظ شود، باید از عبارت `SFX and source dialogue only` استفاده شود.
- اگر فقط افکت صوتی لازم است، `SFX only` کافی است.
- اگر intro قبل از ویدیوی اصلی اضافه می‌شود، باید زمان intro از کل runtime کم شود.
- این skill برای ساخت صحنه کاملاً جدید نیست؛ برای تغییر ویدیوهای موجود است.
