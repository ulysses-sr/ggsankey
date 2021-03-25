
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ggsankey

<!-- badges: start -->

<!-- badges: end -->

The goal of ggsankey is to make beautiful sankey, alluvial and sankey
bump plots i `ggplot2`

## Installation

You can install the development version of ggsankey from \`\`github
with:

``` r
# install.packages("devtools")
devtools::install_github("davidsjoberg/ggbump")
```

## Example

### geom\_sankey

A basic sankey plot that shows how dimensions are linked.

``` r
library(ggsankey)
library(tidyverse)
library(hablar)

df <- mtcars %>%
  make_long(cyl, vs, am, gear, carb)

ggplot(df, aes(x = x, 
               next_x = next_x, 
               node = node, 
               next_node = next_node,
               fill = factor(node))) +
  geom_sankey()
```

<img src="man/figures/README-example-1.png" width="100%" />

And by adding a little pimp and labels with `geom_sankey_label` which
places labels in the center of nodes if given the same aestethics.
`ggsunkey` also comes with custom minimalistic themes that can be
used.

``` r
ggplot(df, aes(x = x, next_x = next_x, node = node, next_node = next_node, fill = factor(node), label = node)) +
  geom_sankey(flow.alpha = .6) +
  geom_sankey_text(size = 3, color = "white") +
  scale_fill_viridis_d() +
  theme_sankey(base_size = 18) +
  labs(x = NULL) +
  theme(legend.position = "none",
        plot.title = element_text(hjust = .5)) +
  ggtitle("Car features")
```

<img src="man/figures/README-sankey-1.png" width="100%" />

### geom\_alluvial

Alluvial plots are very similiar to sankey plots but have no spaces
between nodes and start at y = 0 instead being centered around the
x-axis.

``` r
ggplot(df, aes(x = x, next_x = next_x, node = node, next_node = next_node, fill = factor(node), label = node)) +
  geom_alluvial(flow.alpha = .6) +
  geom_alluvial_text(size = 3, color = "white") +
  scale_fill_viridis_d() +
  theme_alluvial(base_size = 18) +
  labs(x = NULL) +
  theme(legend.position = "none",
        plot.title = element_text(hjust = .5)) +
  ggtitle("Car features")
```

<img src="man/figures/README-alluvial-1.png" width="100%" />

### geom\_sankey\_bump

Sankey bump plots is mix between bump plots and sankey and mostly useful
for time series. When a group becomes larger than another it bumps above
it.

``` r
# install.packages("gapminder")
library(gapminder)

df <- gapminder %>%
  group_by(continent, year) %>%
  summarise(gdp = (sum_(pop * gdpPercap)/1e9) %>% round(0), .groups = "keep") %>%
  ungroup()

ggplot(df, aes(x = year,
               node = continent,
               fill = continent,
               value = gdp)) +
  geom_sankey_bump(space = 0, type = "alluvial", color = "transparent", smooth = 6) +
  scale_fill_viridis_d(option = "A", alpha = .8) +
  theme_sankey_bump(base_size = 16) +
  labs(x = NULL,
       y = "GDP ($ bn)",
       fill = NULL,
       color = NULL) +
  theme(legend.position = "bottom") +
  labs(title = "GDP development per continent")
```

<img src="man/figures/README-sankey_bump-1.png" width="100%" />
