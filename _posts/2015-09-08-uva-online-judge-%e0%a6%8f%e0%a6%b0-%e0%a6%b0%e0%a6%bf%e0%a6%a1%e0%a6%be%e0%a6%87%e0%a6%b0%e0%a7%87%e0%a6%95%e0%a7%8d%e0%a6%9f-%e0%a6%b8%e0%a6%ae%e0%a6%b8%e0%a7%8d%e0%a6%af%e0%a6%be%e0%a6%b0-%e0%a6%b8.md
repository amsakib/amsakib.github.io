---
layout: post
title: UVA Online Judge এর রিডাইরেক্ট সমস্যার সমাধান
date: 2015-09-08 20:47
author: amsakib
comments: true
categories: [Browser Problem, Google Chrome, Mozilla Firefox, Tricks]
---
যারা <a href="http://uva.onlinejudge.org" target="_blank">UVA Online Judge</a> নিয়মিত ব্যবহার করেন এবং এজন্য Remember Me অপশনটি চেক করে রাখেন তারা প্রায়শই একটি সমস্যায় পড়েন। সেটি হলো নিচের মতো করে দেখানো যে uva.onlinejudge.org is not redirecting properly. অথবা This page is redirecting infinitely.

<a href="http://amsakib.cf/blog/wp-content/uploads/2015/09/11996273_469436679895840_2088166614_o.jpg"><img class="aligncenter size-full wp-image-81" src="http://amsakib.cf/blog/wp-content/uploads/2015/09/11996273_469436679895840_2088166614_o.jpg" alt="This page is in infinite loop." width="819" height="414" /></a>

মূলত ব্রাউজার অটো আপডেট হলে এই সমস্যাটি হয়ে থাকে। এই সমস্যার সমাধান খুব সহজেই করা যায়। আর এটি হলো আপনার ব্রাউজার থেকে শুধু মাত্র onlinejudge.org এর জন্য যে কুকিগুলো জমা হয়ে থাকে সেই কুকিগুলো মুছে ফেলতে হবে। কুকি মোছার জন্য নিচের ধাপগুলো অনুসরণ করুন।
<h2>Google Chrome এর জন্যঃ</h2>
১। প্রথমে ব্রাউজারের Address Bar এ chrome://settings/content লিখে এন্টার দিন। নিচের মতো আসবে।

<a href="http://amsakib.cf/blog/wp-content/uploads/2015/09/chrome_1.png"><img class="aligncenter size-full wp-image-82" src="http://amsakib.cf/blog/wp-content/uploads/2015/09/chrome_1.png" alt="Content Settings" width="666" height="622" /></a>

২। এবার All cookies and site data.. বাটনটিতে ক্লিক করুন। তাহলে নিচের মতো করে আসবে।

<a href="http://amsakib.cf/blog/wp-content/uploads/2015/09/chrome_2.png"><img class="aligncenter size-full wp-image-83" src="http://amsakib.cf/blog/wp-content/uploads/2015/09/chrome_2.png" alt="All cookies and site data" width="731" height="586" /></a>

৩। এবার উপরের ডান পাশে সার্চ করার ঘরে লিখুন onlinejudge.org। এতে ফিল্টার হয়ে নিচের ২টি রেজাল্ট আসবে।

<a href="http://amsakib.cf/blog/wp-content/uploads/2015/09/chrome_3.png"><img class="aligncenter size-full wp-image-84" src="http://amsakib.cf/blog/wp-content/uploads/2015/09/chrome_3.png" alt="Filter cookies" width="795" height="588" /></a>

৪। এবার সাইট ২টির উপর মাউস নিয়ে গেলে ডানপাশে x চিহ্ন দেখা হবে। ওটাতে ক্লিক করে কুকি মুছে ফেলুন। এবার Done এ ক্লিক করুন। আশা করি গুগল ক্রমে আবার UVA Online Judge ভিজিট করতে পারবেন।
<h2>Mozilla Firefox এর জন্যঃ</h2>
১। ব্রাউজারের Address Bar এ about:preferences#privacy লিখে এন্টার দিন।যে পেজ আসবে সেখানে History অংশে remove individual cookies এ ক্লিক করুন।  ( পুরাতন ভার্শন থেকে Options &gt; Privacy , তাপর Firefox Will থেকে Use custom settings for history সিলেক্ট করতে হবে। তারপর Show Cookies বাটন ভিজিবল হবে। )

<a href="http://amsakib.cf/blog/wp-content/uploads/2015/09/firefox_1.png"><img class="aligncenter size-full wp-image-87" src="http://amsakib.cf/blog/wp-content/uploads/2015/09/firefox_1.png" alt="firefox_1" width="805" height="147" /></a>

২। Cookies window ওপেন হবে। এখানে সার্চ করুন onlinejudge.org লিখে।

<a href="http://amsakib.cf/blog/wp-content/uploads/2015/09/cookies.png"><img class="aligncenter size-full wp-image-88" src="http://amsakib.cf/blog/wp-content/uploads/2015/09/cookies.png" alt="cookies" width="574" height="574" /></a>

৩। উপরে যে কুকিগুলো আসছে সেগুলো সিলেক্ট করে Remove Selected এ ক্লিক করুন।

আশা করি Firefox এও এবার UVA Online Judge ওপেন হবে।

অন্যান্য ব্রাউজারেও একইভাবে Settings থেকে শুধুমাত্র onlinejudge.org এর জন্য cookie গুলো মুছে ফেললেই ওয়েবসাইটটি আবার ভিজিট করা যাবে।
