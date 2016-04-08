---
title: Smarty if
date: 2016-04-08 15:28:27
tags: smarty
---


来源：[让一切容易学会--易百](http://www.yiibai.com/smarty/smarty_ifelseif.html)


### if elseif else ###
Smarty 中的 if 语句和 php 中的 if 语句一样灵活易用，并增加了几个特性以适宜模板引擎. if 必须于 /if 成对出现. 可以使用 else 和 elseif 子句. 可以使用以下条件修饰词：eq、ne、neq、gt、lt、lte、le、gte、ge、is even、is odd、is not even、is not odd、not、mod、div by、even by、odd by、==、!=、>、<、<=、>=. 使用这些修饰词时必须和变量或常量用空格格开

<!--more-->

    {if $name eq "Fred"}
    	Welcome Sir.
    {elseif $name eq "Wilma"}
    	Welcome Ma'am.
    {else}
    	Welcome, whatever you are.
    {/if}
    
    {* an example with "or" logic *}
    {if $name eq "Fred" or $name eq "Wilma"}
    	...
    {/if}
    
    {* same as above *}
    {if $name == "Fred" || $name == "Wilma"}
    	...
    {/if}
    
    {* the following syntax will NOT work, conditional qualifiers
     must be separated from surrounding elements by spaces *}
    {if $name=="Fred" || $name=="Wilma"}
    	...
    {/if}
    
    
    {* parenthesis are allowed *}
    {if ( $amount < 0 or $amount > 1000 ) and $volume >= #minVolAmt#}
    	...
    {/if}
    
    {* you can also embed php function calls *}
    {if count($var) gt 0}
    	...
    {/if}
    
    {* test if values are even or odd *}
    {if $var is even}
    	...
    {/if}
    {if $var is odd}
    	...
    {/if}
    {if $var is not odd}
    	...
    {/if}
    
    {* test if var is divisible by 4 *}
    {if $var is div by 4}
    	...
    {/if}
    
    {* test if var is even, grouped by two. i.e.,
    0=even, 1=even, 2=odd, 3=odd, 4=even, 5=even, etc. *}
    {if $var is even by 2}
    	...
    {/if}
    
    {* 0=even, 1=even, 2=even, 3=odd, 4=odd, 5=odd, etc. *}
    {if $var is even by 3}
    	...
    {/if}
