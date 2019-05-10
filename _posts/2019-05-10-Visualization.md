---
layout: post
title:  "Visualization :: plot_ly(slider)"
date:   2019-05-10
categories: Visualization
permalink: Visualization
tags: Visualization R
use_math: true

# author
author: Kipoong Kim
---

<!-- more -->

### Visualization with plot_ly

```R
library(dplyr)
library(ggplot2)
library(plotly)
library(gganimate)

n <- 150; sd <- seq(0.1, 0.5, by=0.05)
df <- data.frame(group=rep( paste0("group", 1:3), each=n*length(sd)),
                 mu1=rep( c(0:2), each=n*length(sd) ), 
                 mu2=rep( c(0,1,0), each=n*length(sd) ),
                 SD=rep(sd, each=n)) %>% 
    rowwise() %>% 
    mutate( x1=rnorm(1, mu1, SD),
            x2=rnorm(1, mu2, SD)) 

# \begin gganimate --------------------------------------------------------
df %>% ggplot(aes(x1, x2, colour=group)) + 
    geom_point() + 
    theme_bw() + 
    transition_time(SD)
# \end gganimate ----------------------------------------------------------

# \begin plot_ly_slider ---------------------------------------------------
p <- df %>%
    arrange(group, SD, sqrt(x1^2+x2^2)) %>% 
    plot_ly(
        x = ~x1, 
        y = ~x2, 
        # size = ~pop, 
        color = ~group, 
        frame = ~SD, 
        # text = ~country, 
        hoverinfo = "text",
        type = 'scatter',
        mode = 'markers'
    )
# \end plot_ly_slider -----------------------------------------------------
```



<iframe height='400' scrolling='yes' title='Fancy Animated SVG Menu' src='../img/Visualization_cluster.html' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 70%;'></iframe>

