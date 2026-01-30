---
title: "お問い合わせ"
date: 2026-01-03
draft: false
---

以下のフォームよりお問い合わせください。通常2〜3営業日以内にご返信いたします。

<form id="contact-form" class="contact-form">
    <div class="form-group">
        <label for="name">お名前</label>
        <input type="text" id="name" name="name" placeholder="山田 太郎" required>
    </div>
    <div class="form-group">
        <label for="email">メールアドレス</label>
        <input type="email" id="email" name="email" placeholder="example@stellorbit.net" required>
    </div>
    <div class="form-group">
        <label for="message">メッセージ</label>
        <textarea id="message" name="message" rows="5" placeholder="お問い合わせ内容をご記入ください" required></textarea>
    </div>
    <button type="submit" id="submit-btn">送信する</button>
    <p id="form-status" class="form-status"></p>
</form>

<script>
document.getElementById('contact-form').addEventListener('submit', async (e) => {
    e.preventDefault();
    
    const btn = document.getElementById('submit-btn');
    const status = document.getElementById('form-status');
    const cloudFunctionUrl = 'https://contact-form-1095497309504.asia-northeast1.run.app';

    // 送信中の状態
    btn.disabled = true;
    btn.innerText = '送信中...';
    status.innerText = '';

    const formData = {
        name: document.getElementById('name').value,
        email: document.getElementById('email').value,
        message: document.getElementById('message').value
    };

    try {
        const response = await fetch(cloudFunctionUrl, {
            method: 'POST',
            mode: 'cors', // CORSを明示
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(formData)
        });

        if (response.ok) {
            status.innerText = '✅ お問い合わせありがとうございました。送信が完了しました。';
            status.style.color = '#28a745';
            document.getElementById('contact-form').reset();
        } else {
            throw new Error('送信エラー');
        }
    } catch (error) {
        status.innerText = '❌ 送信に失敗しました。時間をおいて再度お試しください。';
        status.style.color = '#dc3545';
    } finally {
        btn.disabled = false;
        btn.innerText = '送信する';
    }
});
</script>