---
title: contentEditable
date: 2017-07-05 18:53:34
tags:
    - CSS3
    - placeholder
---
* HTML
````
    <div contentEditable=true data-ph="My Placeholder String"></div>
````
* CSS
````
    [contentEditable=true]{
    border:1px solid grey;  
    }

    [contentEditable=true]:empty:not(:focus):before{
    content:attr(data-ph);
    color:grey;
    font-style:italic;
    }
````