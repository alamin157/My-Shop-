# <!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>আমার দোকান</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
<symbol id="tee" viewBox="0 0 100 120"><path d="M30 10 10 28l10 14 8-6v72h44V36l8 6 10-14L70 10Q50 24 30 10Z" fill="currentColor"/></symbol>
<symbol id="dress" viewBox="0 0 100 120"><path d="M38 8h24l-2 32 18 72H22l18-72Z" fill="currentColor"/></symbol>
<symbol id="pants" viewBox="0 0 100 120"><path d="M30 8h40l4 104H54L50 44l-4 68H26Z" fill="currentColor"/></symbol>
</svg>

<header>
<div class="brand">
<svg viewBox="0 0 64 64" aria-hidden="true"><rect width="64" height="64" rx="14" fill="#3B3FD8"/><path d="M20 14v30h14M30 50c8 0 14-6 14-14V14" fill="none" stroke="#fff" stroke-width="5" stroke-linecap="round" stroke-linejoin="round"/><circle cx="44" cy="50" r="3.5" fill="#fff"/></svg>
<span id="shop"></span>
</div>
<button class="bag" id="open">ব্যাগ (<span id="count">0</span>)</button>
</header>

<section class="hero">
<h1>প্রতিদিনের জন্য আরামদায়ক পোশাক</h1>
<p>১০০% কটন ও লিনেন। সারা বাংলাদেশে হোম ডেলিভারি। ক্যাশ অন ডেলিভারি, বিকাশ ও নগদ গ্রহণযোগ্য।</p>
</section>

<div class="filters" id="filters"></div>
<main class="grid" id="grid"></main>
<footer id="foot"></footer>

<div class="scrim" id="scrim"></div>
<aside class="panel" id="panel" role="dialog" aria-modal="true"></aside>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2.45.4/dist/umd/supabase.js"></script>