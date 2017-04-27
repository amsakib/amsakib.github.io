---
layout: post
title: স্ট্রংলি কানেক্টেড কম্পোনেন্ট ( SCC )
date: 2015-10-20
author: amsakib
comments: true
categories: [Algorithm, Algorithms, DFS, SCC]
---
<p style="text-align: justify;">Strongly Connected Component হলো একটি ডিরেক্টড গ্রাফ G = (V, E) এর ভার্টিক্স এর সেট C∈V যেখোনে C এর প্রতিটি ভার্টেক্স u থেকে v তে যাবার একটি পথ আছে এবং v থেকে u তে যাবারও পথ আছে। অর্থাৎ, u এবং v নোড দুটি থেকে পরষ্পরকে ভিজিট করা যাবে। বিষয়টা ভালোভাবে বোঝার জন্য নিজের গ্রাফটি দেখা যাকঃ</p>


[caption id="attachment_94" align="aligncenter" width="611"]<a href="http://amsakib.cf/blog/wp-content/uploads/2015/10/SampleGraph.png"><img class="size-full wp-image-94" src="http://amsakib.cf/blog/wp-content/uploads/2015/10/SampleGraph.png" alt="Sample Graph" width="611" height="169" /></a> Sample Graph[/caption]
<p style="text-align: justify;">উপরের গ্রাফে ভার্টেক্সের সেট হলো {1, 2, 3, 4, 5, 6, 7, 8}। এর মধ্যে ১ নম্বর নোড (ভার্টেক্স) থেকে ২ নম্বর নোডে যাওয়া যায়। আবার ২ নম্বর নোড থেকেও ১ নম্বর নোডে যাওয়া যায় ২-&gt;৪-&gt;৩-&gt;১ এই পথের মাধ্যমে। এখানে ১, ২, ৩, ৪ এর প্রত্যেকটি নোড থেকেই কিন্তু প্রত্যেকটি নোডে পৌঁছানো সম্ভব। কিন্তু এদের যে কোনো নোড থেকে ৫ এ পৌঁছানো গেলেও, ৫ থেকে এই সকল নোডে পৌঁছানো সম্ভব না। তাই ১, ২, ৩, ৪ নোড এর সেট হলো একটি স্ট্রংলি কানেক্টেড কম্পোনেন্ট এর সেট। আর {6, 7, 8} ও একটি স্ট্রংলি কানেক্টেড কম্পোনেন্ট সেট । আর {5} হলো একটি নোড এর কানেক্টেড কম্পোনেন্ট সেট।</p>
<p style="text-align: justify;">স্ট্রংলি কানেকটেড কম্পোনেন্ট বের করার দুটি জনপ্রিয় অ্যালগোরিদম হলোঃ</p>
<p style="text-align: justify;">১। টারজানের অ্যালগরিদম [Tarjan’s Algo ]</p>
<p style="text-align: justify;">২। কোসরাজু-শাহরির [Kosaraju-Sharir] -এর অ্যালগরিদম</p>
<p style="text-align: justify;">এই পোস্টে আমি কোসারাজু-শাহরির এর অ্যালগরিদমের মাধ্যমে কোনো ডিরেক্টটেড গ্রাফে স্ট্রংলি কানেকটেড কম্পোনেন্টগুলো খুঁজে রেব করা আলোচনা করবো।</p>
<p style="text-align: justify;">কোসারাজুর অ্যালগরিদমে দুটো DFS ব্যবহার করা হয়। প্রথম DFS এর মাধ্যমে নোডগুলোকে পোস্ট অর্ডারে সর্ট করা হয়। এবং দ্বিতীয় DFS এর মাধ্যমে মূলত ঐ সর্টেড নোডগুলোতে DFS চালানো হয়, তবে এক্ষেত্রে DFS টা চালানো হয় মূল গ্রাফের বিপরীত (reverse) গ্রাফে। রিভার্স গ্রাফে ডিরেকশনগুলো মূল গ্রাফের বিপরীত থাকে। তাহলে উপরের গ্রাফটির রিভার্স গ্রাফ হবেঃ</p>


[caption id="attachment_95" align="aligncenter" width="611"]<a href="http://amsakib.cf/blog/wp-content/uploads/2015/10/ReverseGraph.png"><img class="size-full wp-image-95" src="http://amsakib.cf/blog/wp-content/uploads/2015/10/ReverseGraph.png" alt="Reverse Graph" width="611" height="169" /></a> Reverse Graph[/caption]
<p style="text-align: justify;">এখানে গ্রাফ রিভার্জ করা কঠিন কোনো বিষয় না। গ্রাফ Adjancy List আকারে ইনপুট নেবার সময় আরেকটি লিস্টে বিপরীত দিকের গ্রাফ তৈরী করলেই Reverse Graph পাওয়া যাবে।</p>
<p style="text-align: justify;">এবারে Kosaraju র অ্যালগরিদমের স্টেপগুলো বিস্তারিত দেখি।</p>
<p style="text-align: justify;">১। মূল গ্রাফের প্রতিটি নোড থেকে প্রথম DFS চালানো হবে যদি সেই নোডটি ভিজিটেড না হয়ে থাকে। এজন্য একটি visited অ্যারে নিতে হবে।</p>
<p style="text-align: justify;">২। প্রথম DFS এর ভেতর প্রথমেই যে নোডটিকে visited করতে হবে এবং এর adjacent নোডগুলোকেও ভিজিট করতে হবে। সবগুলো নোড ভিজিট করা হয়ে গেলে DFS ফাংশনটির একদম শেষে ঐ নোডটিকে একটি লিস্টে (L) অ্যাড করে দিতে হবে।</p>
<p style="text-align: justify;">৩। পোস্ট অর্ডারে আমরা নোড গুলো লিস্টে (L) পাবো। লিস্টটাকে রিভার্স করে নিয়ে আমাদেরকে পরবর্তী কাজ করতে হবে।</p>
<p style="text-align: justify;">৪। এবার আমরা এই লিস্ট টা থেকে একটি করে নোড নিবো আর সেই নোডটি থেকে দ্বিতীয় DFS চালাবো। তবে এবার আমরা DFS চালাবো রিভার্স করা গ্রাফে এবং এখানে আমরা আগের visited অ্যারেটি ব্যবহার করতে পারি। যদি নোড ভিজিটেড থাকে তাহলে সেই নোড থেকে DFS চালানো হবে। একটি DFS এ যে সকল নোড এ ভিজিট করা যাবে তারাই হবে একটি কানেক্টেড কম্পোনেন্টের সেট।</p>
<p style="text-align: justify;">পুরো অ্যালগরিদমটি কোড আকারে দিলাম। কোডটি দেখলে এবং প্রতিটি ধাপ নিজে নিজে খাতায় illustrate করলে বুঝা যাবে যে কোডটি কেন কাজ করতেছে।</p>

<pre class="lang:c++ decode:true " title="Kosaraju's Algorithm">#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;

#include &lt;iostream&gt;
#include &lt;algorithm&gt;
#include &lt;vector&gt;

using namespace std;

#define MX 5001

vector&lt;int&gt; List, graph[MX], reverse_graph[MX], scc[MX];
int visited[MX];

// First DFS that creates POST ORDER list
void dfs1(int u)
{
    visited[u] = 1;
    int sz = graph[u].size();
    for(int i=0; i&lt;sz; i++) {
        if(!visited[graph[u][i]]) {
            dfs1(graph[u][i]);
        }
    }
    // Add to list after all adjacent nodes are finished visited
    List.push_back(u);
}

// Second DFS finds a set of strongly connected components
void dfs2(int u, int sccNo)
{
    visited[u] = 0;
    // add current node to the same component list
    scc[sccNo].push_back(u);
    int sz = reverse_graph[u].size();
    for(int i=0; i&lt;sz; i++) {
        if(visited[reverse_graph[u][i]]) {
            dfs2(reverse_graph[u][i], sccNo);
        }
    }
}

// this function calls the first and second DFS
// and returns the total number of strongly connected components set
int StronglyConnectedComponents(int n)
{
    memset(visited, 0, sizeof(int)*(n+1));
    int i = 0;
    for(i=1; i&lt;=n; i++) {
        if(!visited[i]) {
            dfs1(i);
        }
    }

    // stores total number of Strongly Connected Components Set
    int sccNo = 0;

    // do not need to manually reverse the list
    // just pick the nodes from the last element of the list
    for(i=List.size()-1; i&gt;=0; i--) {
        if(visited[List[i]]) { // If node is visited
            dfs2(List[i], sccNo); // call to the second DFS
            sccNo++;
        }
    }
    return sccNo;
}

int main()
{
    /*
    INPUT DATA for given example
    -----------------------------
    8 10
    1 2
    1 3
    2 4
    4 3
    3 1
    3 5
    5 6
    6 8
    8 7
    7 6
    */
    int nodes, edges, u, v, i, j, n;
    scanf("%d %d", &amp;nodes, &amp;edges);

    for(i=0; i&lt;edges; i++) {
        scanf("%d %d", &amp;u, &amp;v);

        graph[u].push_back(v);
        reverse_graph[v].push_back(u);
    }

    n = StronglyConnectedComponents(nodes);
    printf("%d Strongly Connected Components found in the graph.\n", n);
    for(i=0; i&lt;n; i++) {
        printf("SCC[%d]:", i+1);
        for(j=0; j&lt;(int)scc[i].size(); j++) {
            printf(" %d", scc[i][j]);
        }
        printf("\n");
    }

    return 0;
}
</pre>
<span style="text-decoration: underline;"><strong> প্রাকটিস প্রবলেমঃ</strong></span>

1. <a href="https://uva.onlinejudge.org/index.php?option=com_onlinejudge&amp;Itemid=8&amp;page=show_problem&amp;category=24&amp;problem=183&amp;mosmsg=Submission+received+with+ID+16299379" target="_blank">UVA 247 - Calling Cirlcles</a>
