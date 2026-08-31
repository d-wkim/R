**Set up**: Install packages and load libraries.

```R


setwd("G:/My Drive/github/d-wkim/network_meta-analysis/")

packages <- c(
  "googledrive",
  "googlesheets4",
  "readxl",
  "metafor",
  "dplyr",
  "meta",
  "ggplot2",
  "ggpubr",
  "ggplotify",
  "ggplot2",
  "tidyverse",
  "devtools",
  "netmeta",
  "rgl"
)


#install.packages(packages, dependencies = TRUE)

lapply(packages, library, character.only = TRUE)

```

Custom R function for running **network meta-analysis**.

```R

network_meta_analysis <- function(df, sm = "") {
  
  data_path <- paste0("G:/My Drive/github/d-wkim/network_meta-analysis/3_meta-analysis/R/analysis/",df, ".csv")
  
  data <- read_csv(data_path)
  
  m.netmeta <- netmeta(
		TE = smd, 
		seTE = se,
		treat1 = treat1,
		treat2 = treat2,
		studlab = study,
		data = data,
		sm = paste0(sm),
		common = FALSE, 
		random = TRUE,
		reference.group = "BPTB",
		details.chkmultiarm = TRUE,
		sep.trts = " vs "
	)

	txt = paste0("G:/My Drive/github/d-wkim/network_meta-analysis/3_meta-analysis/R/analysis/results/",df, ".txt")

	# print and save network meta-analysis results
	
	sink(file = txt)
	print(m.netmeta)
	sink()

	labels = m.netmeta$trts

	svg = paste0("G:/My Drive/github/d-wkim/network_meta-analysis/4_manuscript/network_meta-analysis/tables_and_figures/figures/fig_3/", df, ".svg")
	svg(file = svg, height = 5, width = 5)

	network_graph <- netgraph(m.netmeta, labels = labels, dim = "2d", scale = 1.1)
	print(network_graph)
	dev.off()
	
  assign(paste0(df), m.netmeta, envir = .GlobalEnv) # Assign the outcome names as variables of the results
  
  header <- c("SMD", "ci.lb", "ci.ub")
  ES <- m.netmeta$TE.nma.random
  ci.lb <- m.netmeta$lower.nma.random
  ci.ub <- m.netmeta$upper.nma.random
  
  dat <- rbind(header,
              ES,
              ci.lb,
              ci.ub
  )
  
  write.csv(dat, file = paste0("G:/My Drive/github/d-wkim/network_meta-analysis/4_manuscript/network_meta-analysis/tables_and_figures/supplementary_tables/",df,".csv"))
}

```

**Function applied** to a vector list of dataset.

```R

continuous = c("ikdc_subjective", "lysholm", "tegner", "instrumented_laxity")
lapply(continuous, sm = "SMD", network_meta_analysis)
```

```R
dichotomous = c("pivot_shift_1", "pivot_shift_2", "lachman_1", "lachman_2", "graft_failure")
lapply(dichotomous, sm = "OR", network_meta_analysis)
```
