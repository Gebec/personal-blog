---
title: "Custom components benchmark"
date: 2021-12-06
author: "Gebec"
techs: ["au"]
tags: ["Aurelia", "Guide"]
categories: ["Aurelia", "Tips"]
draft: false
---

When our web app grows we will more often try to optimize the speed of every page. There are areas that we can influence - ineffective iterations, data caching, proper event handling,... But there is something we can't touch and influence - how the framework and its methods are optimized. And if the framework offers more options how to do one thing, we may ask which one is more performant.


In this blog post I focus on Aurelia's ways to include custom components. All possibilities that Aurelia offers were summarized in one of previous [blog post](/blog/2021-12-04-custom-aurelia-components/). But, which approach is the fastest and which should not be used at all?


### Setting up the tests
For benchmarking I used locally installed project, but to show, how the code looks like, I prepared showcase on Plunker:
- custom component tag ([source code](https://plnkr.co/edit/dpt0tjpZ5rzlG3jN))
- custom component with as-element attribute ([source code](https://plnkr.co/edit/RDsw7VoCjDoMik1J))
- custom component tag with containerless attribute ([source code](https://plnkr.co/edit/mOmiLtC98fK2uzns))
- compose component ([source code](https://plnkr.co/edit/XIrNeH6UpTbYLDhn))
- compose component with as-element attribute ([source code](https://plnkr.co/edit/f06orc7KfmbhN5nV))
- compose component with containerless attribute ([source code](https://plnkr.co/edit/yrT4H0vK9UQmsmQB))

Locally I run node 14.17.4 and aurelia-bootstrapper as dependency in version 2.3.3.

Every custom component was measured 10 times in Chrome browser in anonymous window. After every measurement I closed the browser window to reduce the possibility of caching or any other effect. The exact times were deducted from Chrome dev tools performance tab.

### How fast are the custom components

#### Custom component tag

Measured times:

<table>
  <tr>
    <th></th>
    <th>#1</th>
    <th>#2</th>
    <th>#3</th>
    <th>#4</th>
    <th>#5</th>
    <th>#6</th>
    <th>#7</th>
    <th>#8</th>
    <th>#9</th>
    <th>#10</th>
    <th>Average [s]</th>

    <th>Deviation</td>
  </tr>
  <tr>
    <td>Measured times [s]</td>

    <td>1.08</td>
    <td>0.78</td>
    <td>0.64</td>
    <td>0.76</td>
    <td>1.34</td>
    <td>1.31</td>
    <td>1.27</td>
    <td>1.39</td>
    <td>0.70</td>
    <td>0.80</td>
    </tr>
</table>

#### Custom component with as-element attribute

Measured times:

<table>
  <tr>
    <td colspan="10">Measurements</td>
    <td>Average</td>
    <td>Deviation</td>

  </tr>
  <tr>
    <td>0.91</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>0.89</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>0.74</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>0.74</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>0.69</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>0.77</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>0.70</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>1.75</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>1.55</td>
    <td></td>
    <td></td>
<tr></tr>
    <td>1.46</td>
    <td></td>
    <td></td>
<tr></tr>
  </tr>
</table>

#### Custom component with containerless attribute

Measured times:

<table>
  <tr>
    <td colspan="10">Measurements</td>
    <td>Average</td>
    <td>Deviation</td>
  </tr>
  <tr>
    <td></td><td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
    <td></td>
    <td></td>
    <td></td>
<tr></tr>
  </tr>
</table>

#### Compose tag

Measured times:

<table>
  <tr>
    <th colspan="10">Measurements</th>
    <th>Average</th>
    <th>Deviation</td>
  </tr>

  <tr>
    <td>2.21</td>
    <td></td>
    <td></td>
  </tr>

  <tr>
    <td>1.78</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>2.75</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>2.07</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>2.65</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>1.73</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>2.92</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>1.64</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>1.80</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>2.90</td>
    <td></td>
    <td></td>
  </tr>
</table>

#### Compose tag with as-element attribute

Measured times:

<table>
  <tr>
    <td colspan="10">Measurements</td>
    <td>Average</td>
    <td>Deviation</td>
  </tr>
  <tr>
    <td>1.71</td>
    <td>1.74</td>
    <td>2.05</td>
    <td>1.91</td>
    <td>2.86</td>
    <td>2.12</td>
    <td>2.02</td>
    <td>1.92</td>
    <td>1.82</td>
    <td>2.34</td>
    <td></td>
    <td></td>
  </tr>
</table>

#### Compose tag with containerless

Measured times:

<table>
  <tr>
    <td colspan="10">Measurements</td>
    <td>Average</td>
    <td>Deviation</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
