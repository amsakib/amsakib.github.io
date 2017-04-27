---
layout: post
title: Operating Systems: Process Scheduling
date: 2015-05-30 13:37
author: amsakib
comments: true
categories: [Operating Systems, Study Notes]
---
<p style="text-align: justify;">Single-Processor সিস্টেমে একই সময়ে শুধুমাত্র ১টি প্রসেস রান করতে পারে। অন্য প্রসেসগুলোকে তখন CPU free না হওয়া পর্যন্ত অপেক্ষা করতে হয়ে। যেহেতু আমাদেরকে সবসময় CUP Utilization ম্যাক্সিমাম রাখার চেষ্টা করতে হয়, তাই কোন প্রসেসের পর কোন প্রসেস রান করবে তা সিন্ধান্ত নিতে হয়। এবং এটি Process Scheduling এর মাধ্যমে নেওয়া হয়।</p>

<h2 style="text-align: justify;">CPU-I/O Burst Cycle</h2>
<p style="text-align: justify;">একটি প্রসেস এক্সিকিউশনের সময় CPU execution এবং I/O wait এর cycle দ্বার গঠিত। প্রসেসগুলো এই দুটি স্টেটের পরিবর্তনের মাঝে এক্সিকিউট হতে থাকে। প্রসেস এক্সিকিউশন CPU burst এর মাধ্যমে শুরু হয়ে থাকে। এবং এর পরে I/O burst, এরপরে আবার CPU burst এবং এরপর আবার I/O burst .. এভাবে চলতে থাকে। এবং সবার শেষে CPU burst এর মাধ্যমে প্রসেস টারমিনেশনের জন্য রিকুয়েসট করে থাকে। এই পুরো সাইকেলটা এভাবে চিত্রের মাধ্যমে দেখানো যেতে পারেঃ</p>


[caption id="" align="aligncenter" width="338"]<img class="" src="http://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_01_CPU_BurstCycle.jpg" alt="Alternating sequence of CPU and I/O bursts" width="338" height="632" /> Alternating sequence of CPU and I/O bursts[/caption]
<p style="text-align: justify;">I/O Bound প্রোগ্রামগুলোতে সাধারনত অনেকগুলো ছোট ছোট CPU burst থাকে। আবার CPU Bound প্রোগ্রামগুলোতে অল্প সংখ্যক বড় CPU Bursts থাকে।</p>

<h2 style="text-align: justify;">CPU Scheduler</h2>
<p style="text-align: justify;">যখনি CPU নিষ্ক্রিয় (idle) অবস্থায় থাকে তখন অপারেটিং সিস্টেমকে অবশ্যই কোনো একটি প্রোসেসকে Ready Queue থেকে এক্সিকিউশন করার জন্য সিলেক্ট করতে হবে। এই সিলেকশনটি short-term scheduler করে থাকে। সিডিউলারটি একটি প্রসেস সিলেক্ট করে মেমোরিতে লোড করে এবং এর জন্য CPU অ্যালোকেট করে দেয়।</p>
<p style="text-align: justify;">নিচের ৪টি ঘটনার পরিপ্রেক্ষিতে CPU-scheduling এর সিদ্ধান্ত নিতে পারেঃ</p>

<ol style="text-align: justify;">
	<li>যখন একটি প্রসেস running state থেকে waiting state এ সুইচ করে। উদাহরনসরূপ: I/O request অথবা child process এর টারমিনেশনের জন্য অপেক্ষা।</li>
	<li>যখন একটি প্রসেস running state থেকে ready state এ সুইচ করে। উদাহরনঃ যখন ইন্টারাপ্ট ঘটে।</li>
	<li>যখন একটি প্রেসেস waiting state থেকে ready state এ সুইচ করে। উদাহরনঃ I/O টাস্ক শেষ হয়ে যাওয়া।</li>
	<li>যখন একটি প্রসেস শেষ হয়ে যায়।</li>
</ol>
<p style="text-align: justify;">যখন ১ এবং ৪ নম্বর কারনে সিডিউলিং হয় তাকে বলা হয় <strong>nonpreemptive</strong> বা cooperative scheduling আর ২ ও ৩ নম্বর কারনে সিডিউলিং হলে তাকে <strong>preemptive scheduling</strong> বলে।</p>

<h3 style="text-align: justify;">Dispatcher</h3>
<p style="text-align: justify;">CPU scheduling এর আরেকটি উপাদান হলো dispatcher. এই dispatcher হলো একটি মডিউল, short-term scheduler এর মাধ্যমে CPU কে প্রসেস সিলেক্ট করে কন্ট্রোল দিয়ে দিয়ে। এর ফাংশনগুলো হলোঃ</p>

<ul style="text-align: justify;">
	<li>কনটেক্সট সুইচিং</li>
	<li>ইউজার মোড সুইচ করা</li>
	<li>ইউজার প্রোগ্রামের সঠিক যায়গায় চলে যাওয়া অথবা প্রোগ্রামকে রিস্টার্ট করা</li>
</ul>
<p style="text-align: justify;">এই dispatcher কে যতো দ্রুত সম্ভব করতে হয়, যেহেতু এটিকে প্রতিটি প্রসেস সুইচের সময় ইনভোক করতে হয়। Dispatcher এর একটি প্রসেস স্টপ করে আরেকটি প্রসেস চালু করতে যে সময় লাগে তাকে <strong>dispatch latency</strong> বলে।</p>

<h2 style="text-align: justify;">Scheduling Criteria</h2>
<p style="text-align: justify;">CPU Scheduling algorithm গুলোর বিভিন্ন আলাদা আলাদা বৈশিষ্ট্য আছে। বিভিন্ন মাপকাঠির উপর ভিত্তি করে এই সিডিউলিং অ্যালগোরিদমগুলোর কোনটি ভালো, কোনটি সঠিক হবে, কোনটি উপযুক্ত হবে তা নির্ধারন করা হয়। এরকম কয়েকটি মাপকাঠি হলোঃ</p>

<ul style="text-align: justify;">
	<li><strong>CPU Utilization:</strong> সিপিউ কে যতোক্ষন সম্ভব ব্যাস্ত রাখাই আমাদের উদ্দেশ্য। তাত্বিকভাবে CPU utilization 0 থেকে ১০০ শতাংশ পর্যন্ত হতে পারে। কিন্তু বাস্তবে, এটাকে ৪০ (40% - lightly loaded system ) শতাংশ থেকে ৯০ শতাংশ ( highly loaded system) পর্যন্ত রাখাটাই ভালো।</li>
	<li><strong>Throughput:</strong> যদি CPU প্রসেস এক্সিকিউট করার জন্য ব্যাস্ত থাকে তার মানে হলো কাজ হচ্ছে। একক সময় যতো সংখ্যক প্রসেস সম্পন্ন হচ্ছে তাকে throughput বলে।  অনেক বড় প্রসেসর ক্ষেত্রে এটা অতে পারে one process per hour, আবার ছোট প্রসেসগুলোর ক্ষেত্রে এটা হতে পারে ten processes per second.</li>
	<li><strong>Turnaround time:</strong> একটি প্রসেস সাবমিশনের শুরু হতে তা এক্সিকিউশন হওয়া পর্যন্ত মোট সময়েক turnaround time বলে। মূলত এটি waiting time, CPU execution time, I/O operation time সবগুলোর সমষ্টি।</li>
	<li><strong>Waiting time:</strong> একটি প্রসেস যতোক্ষন ready queue তে অপেক্ষমান থাকে সেই সময়কে waiting time বলে।</li>
	<li><strong>Response time:</strong> একটি প্রসেস সাবমিট করা থেকে রেসপন্ড করার আগ পর্যন্ত সময়কে response time বলে।</li>
</ul>
<h2 style="text-align: justify;">Scheduling Algorithms</h2>
<p style="text-align: justify;">বিভিন্ন ধরনের সিডিউলিং অ্যালগোরিদম রয়েছে। এখানে তাদের মধ্যে অল্প কয়েকটি অ্যালগোরিদম নিয়ে আলোচনা করা হলোঃ</p>

<h3 style="text-align: justify;">⇒First-Come, First-Serve Scheduling</h3>
<p style="text-align: justify;">First-Come, First-Serve (FCFS) হলো সবচেয়ে সিম্পল সিপিউ সিডিউলিং অ্যালগোরিদম। এই স্কিমায় প্রথম যে প্রসেসটি আসবে সেই প্রসেসটিকেই প্রসেসর এক্সিকিউট করা শুরু করবে। এর ইম্প্লিমেন্টেশন একটি FIFO queue র মাধ্যমে খুব সহজেই করা যায়। যখনি একটি প্রসেস read queue তে প্রবেশ করবে তখনি এর PCB রেডি কিউর শেষে লিংক হিসেবে থাকবে। যখন CPU ফাঁকা থাকবে তখন সে ready queue এর প্রথমে যে প্রসেসটি রয়েছে সেটির এক্সিকিউশন শুরু করবে এবং এই রানিং প্রসেসটিকে ready queue থেকে মুছে ফেলবে। এই FCFS অ্যালগোরিদমটির কোড খুব সহজেই লেখা সম্ভব এবং সহজেই বোঝা যায়।</p>
<p style="text-align: justify;">তবে FCFS পলিসিতে প্রসেসগুলো গড় অপেক্ষার সময় ( average waiting time ) অনেক বেশি হয়ে থাকে। এটি বোঝার জন্য তিনটি প্রসেস P1, P2 এবং P3 একদম ০ তম সময়ে কিউ তে অ্যরাইভ করেছে। এদের বার্সট টাইম নিচে দেওয়া হলোঃ</p>

<table>
<tbody>
<tr>
<th style="text-align: center;">Process</th>
<th style="text-align: center;">Burst Time</th>
</tr>
<tr>
<td style="text-align: center;">P1</td>
<td style="text-align: center;">24</td>
</tr>
<tr>
<td style="text-align: center;">P2</td>
<td style="text-align: center;">3</td>
</tr>
<tr>
<td style="text-align: center;">P3</td>
<td style="text-align: center;">3</td>
</tr>
</tbody>
</table>
<p style="text-align: justify;">যদি এরা P1, P2, P3 অর্ডারে  আসে তাহলে এদের এক্সিকিউশনের ফলাফল নিচের Gantt Chart এর মাধ্যমে দেখানো যাবেঃ</p>
<p style="text-align: justify;"><a href="http://amsakib.cf/blog/wp-content/uploads/2015/05/fcfs.png"><img class="aligncenter wp-image-65 size-full" src="http://amsakib.cf/blog/wp-content/uploads/2015/05/fcfs.png" alt="Gantt Chart of FCFS" width="611" height="97" /></a>উপরের Gantt Chart থেকে দেখা যাচ্ছে যে P1 কে ০ মিলিসেকেন্ড অপেক্ষা করতে হচ্ছে, P2 কে ২৪ মিলিসেকেন্ড অপেক্ষা করতে হচ্ছে এভং P3 কে ২৭ মিলি সেকেন্ড অপেক্ষা করতে হচ্ছে। তাহলে Average Waiting time হবে = ( 0 + 24 + 27 ) / 3 = 17 milliseconds.</p>
<p style="text-align: justify;">আবার এরা যদি P2, P3, P1 অর্ডারে আসে তাহলে Gantt Chart টি হবেঃ</p>
<p style="text-align: justify;"><a href="http://amsakib.cf/blog/wp-content/uploads/2015/05/FCFS_2.png"><img class="aligncenter size-full wp-image-66" src="http://amsakib.cf/blog/wp-content/uploads/2015/05/FCFS_2.png" alt="FCFS_2" width="606" height="85" /></a>এক্ষেত্রে P2 এর ওয়েটিং টাইম 0 মিলিসেকেন্ড , P3 এর ওয়েটিং টাইম ৩ মিলিসেকেন্ড এবং P1 এর ওয়েটিং টাইম মাত্র ৬ মিলিসেকেন্ড। তাহলে Average Waiting Time = ( 0 + 3 + 6 ) / 3 = 3 milliseconds মাত্র ! অর্থাৎ, FCFS এর average waiting time এদের অর্ডারের উপর নির্ভর করে।</p>

<h3 style="text-align: justify;">⇒Shortest-Job-First Scheduling</h3>
<p style="text-align: justify;">আরও একটি সিডিউলিং অ্যালগরিদম হলো <strong>Shortest-Job-First (SJB) Scheduling Algorithm</strong>. এই অ্যালগোরিদমে CPU পরবর্তীতে যে প্রসেসের burst time কম সেটাকে এক্সিকিউট করা শুরু করে। যদি পরপর দুটি প্রসেসের বার্স্ট টাইম সমান হয় তাহলে তাদের মধ্যে যে কোন একটিকে সিলেক্ট করে টাই ভেঙ্গে ফেলবে। SJB বুঝার জন্য কয়েকটি প্রসেসের বার্স্ট টাইম দেওয়া হলোঃ</p>

<table>
<tbody>
<tr>
<th style="text-align: center;">Process</th>
<th style="text-align: center;">Burst Time</th>
</tr>
<tr>
<td style="text-align: center;">P1</td>
<td style="text-align: center;">6</td>
</tr>
<tr>
<td style="text-align: center;">P2</td>
<td style="text-align: center;">8</td>
</tr>
<tr>
<td style="text-align: center;">P3</td>
<td style="text-align: center;">7</td>
</tr>
<tr>
<td style="text-align: center;">P4</td>
<td style="text-align: center;">3</td>
</tr>
</tbody>
</table>
উপরের চার্ট অনুযায় SJB অ্যালগোরিদম অনুসারে Gantt Chart হবেঃ

<a href="http://amsakib.cf/blog/wp-content/uploads/2015/05/sjb.png"><img class="aligncenter size-full wp-image-70" src="http://amsakib.cf/blog/wp-content/uploads/2015/05/sjb.png" alt="sjb" width="606" height="95" /></a>উপরের Gantt Chart অনুসারে P4 এর ওয়েটিং টাইম ০, p1 এর ওয়েটিং টাইম ৩, P3 এর ওয়েটিং টাইম ৯ এবং p2 এর ওয়েটিং টাইম হবে ১৬. তাহলে Average Waiting Time = ( 0 + 3 + 9 + 16 ) / 4 = 7 milliseconds. এখানে যদি FCFS অ্যালগিরদম ব্যবহার করা হতো তাহলে Average Waiting Time হতো 10.25 milliseconds.

পরবর্তী CPU burst কতো হবে তা সাধারণত পূববর্ত CPU burst এর দৈর্ঘ্যের exponential average এর মাধ্যমে predict করা হয়। Next Burst Time  প্রেডিক্ট করার জন্য ফরমুলাটি হলোঃ

<a href="http://amsakib.cf/blog/wp-content/uploads/2015/05/sjb_formulla.png"><img class="aligncenter size-full wp-image-71" src="http://amsakib.cf/blog/wp-content/uploads/2015/05/sjb_formulla.png" alt="sjb_formulla" width="195" height="34" /></a>এখানে, tn হলো n তম CPU Burst এর দৈর্ঘ্য, Tn হলো পূর্বের বাস্ট টাইম,  a হলে একটি ধ্রুবক যার মান ০ থেকে ১ এর মধ্যে, এবং Tn+1 হলো পরবর্তী প্রেডিকটেড বাস্ট টাইম।
<h3 style="text-align: justify;">⇒Priority Scheduling</h3>
Priority Scheduling , Shortest Job First এর অনুরূপ। তবে এখানে প্রতিটি প্রসেসের Priority দেওয়া থাকবে। যে প্রসেসে Priority বেশি ( lowest integer value ) সেটি আগে এক্সিকিউট হবে।
<h3 style="text-align: justify;">⇒Round-Robin Scheduling</h3>
<strong>Round-Robin (RR) Scheduling Algorithm</strong> টি মূলত টাইম শেয়ারিং অপারেটিং সিস্টেেমের জন্য উপযোগী করে ডিজাইন করা হয়েছে। এ পদ্ধিতিতে একটি ছোট সময়ের ইউনিট থাকে। এই সময়ের ইউনিটকে Time Quantum বলা হয়। এই টাইম কোয়ন্টাম সাধারণত ১০ থেকে ১০০ মিলিসেকেন্ড পর্যন্ত হতে পারে। RR অ্যালগোরিদমে ব্যবহৃত Ready Queue কে Circular Queue হিসেবে ব্যবহার করা হয়।  প্রতিটি প্রসেসে সর্বোচ্চ ১ টাইম কোয়ান্টাম সময় একাবরে এক্সিকিউট করতে পারবে। ১ টাইম কোয়ান্টামের মধ্যে এক্সিকিউশন শেষ না করতে পারলে সেই প্রসেসটি একেবারে Ready Queue র শেষে চলে যাবে। অ্যালগরিদমটি বোঝার জন্য নিচের টেবলের প্রসেসগুলো ও তাদের বার্স্ট টাইম দেখা যাক।
<table>
<tbody>
<tr>
<th style="text-align: center;">Process</th>
<th style="text-align: center;">Burst Time</th>
</tr>
<tr>
<td style="text-align: center;">P1</td>
<td style="text-align: center;">24</td>
</tr>
<tr>
<td style="text-align: center;">P2</td>
<td style="text-align: center;">3</td>
</tr>
<tr>
<td style="text-align: center;">P3</td>
<td style="text-align: center;">3</td>
</tr>
</tbody>
</table>
<p style="text-align: justify;">মনে করা যাক সিপিউটি মোট ৪ মিলি সেকেন্ডের টাইম কোয়ান্টাম ব্যবহার করে। তাহলে P1 প্রথম ৪ সেকেন্ডের জন্য এক্সিকিউট হবে। যেহেতু এর এক্সিকিউশন শেষ হওয়ার জন্য আরও ২০ মিলিসেকেন্ডের প্রয়োজন তাই এটি কিউর শেষে চলে যাবে। এরপর P2 এক্সিিকউট হবে। যেহেতু এর burst time টাইম কোয়্ন্টামের চেয়ে কম সেহেতু আগেই এ শেষ হয়ে যাবে। এবং এরপর P3 আসবে এবং এও ৩ মিলিসেকেন্ড এক্সিকিউট হবে। এরপর ৪ মিলি সেকেন্ড  করে ৫ বার P1 এক্সিকিউট হবে। তাহলে এর জন্য Gantt Chart হবেঃ</p>
<p style="text-align: justify;"><a href="http://amsakib.cf/blog/wp-content/uploads/2015/05/RR.png"><img class="aligncenter size-full wp-image-74" src="http://amsakib.cf/blog/wp-content/uploads/2015/05/RR.png" alt="RR" width="621" height="90" /></a>এখন P1 এর waiting time হবে (10-4) বা 6 মিলিসেকেন্ড, p2 এর ওয়েটিং টাইম 4 মিলিসেকেন্ড এবং P3 এর ওয়েটিং টাইম 7 মিলিসেকেন্ড। তাহলে Average Waiting Time = ( 6 + 4 + 7 ) / 3 = 17 /3 = 5.66 milliseconds.</p>
<p style="text-align: justify;">এই অ্যালগোরিদমটি <a href="http://cs.uttyler.edu/Faculty/Rainwater/COSC3355/Animations/rr.htm" target="_blank">এই খানে এনিমেশনের মাধ্যমে</a> দেখতে পারে। Animation start করার জন্য Play বাটনে ক্লিক করতে হবে। এবং Nonstop Animation এ টিক চিহ্ণ দিতে হবে।</p>
