> [!NOTE]
> **Remember**: goal is to create ***universally*** usable scripts across future projects, not just this one.

Step 1. **Set up**: Install packages and load libraries.

```R

wd <- getwd()
setwd(wd)

data_path <- paste0(wd,"data/")
analysis_path <- paste0(wd,"analysis/")
plot_path <- paste0(wd,"plots/")

packages <- c(
	"netmeta"
)

install.packages(packages, dependencies = TRUE)
lapply(packages, library, character.only = TRUE)

```

Step 2: Custom R function for running **network meta-analysis**.

```R

network_meta_analysis <- function(data_dir, df, sm = "", reference = "") {
  
  data_path <- paste0(data_dir, df, ".csv")  
  data <- read_csv(data_path)
  reference = paste0(reference)
  
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
		reference.group = reference,
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

Function applied ***without*** using lapply.

1. **IKDC subjective**

```R

# ikdc_subjective (k = 26 studies, 5 comparison groups, d.f. = 26 - 5 +1 = 22)
sink(file = "./ikdc_subjective.BPTB.txt")
print(ikdc_subjective.BPTB <- nma("ikdc_subjective", "SMD", reference = "BPTB"))
sink()

sink(file = "./ikdc_subjective.HT.txt")
print(ikdc_subjective.HT <- nma("ikdc_subjective", "SMD", reference = "HT"))
sink()

sink(file = "./ikdc_subjective.QT.txt")
print(ikdc_subjective.QT <- nma("ikdc_subjective", "SMD", reference = "QT"))
sink()

sink(file = "./ikdc_subjective.PLT.txt")
print(ikdc_subjective.PLT <- nma("ikdc_subjective", "SMD", reference = "PLT"))
sink()

#print(ikdc_subjective.AT <- nma("ikdc_subjective", "SMD", reference = "AT"))

sink(file = "./ikdc_subjective.TA.txt")
print(ikdc_subjective.TA <- nma("ikdc_subjective", "SMD", reference = "TA"))
sink()

```

2. **Lysholm**

```R

# lysholm (k = 26 studies, 5 comparison groups, d.f. = 26 - 5 +1 = 22

sink(file = "./lysholm.BPTB.txt")
print(lysholm.BPTB <- nma("lysholm", "SMD", reference = "BPTB"))
sink()

sink(file = "./lysholm.HT.txt")
print(lysholm.HT <- nma("lysholm", "SMD", reference = "HT"))
sink()

sink(file = "./lysholm.QT.txt")
print(lysholm.QT <- nma("lysholm", "SMD", reference = "QT"))
sink()

sink(file = "./lysholm.PLT.txt")
print(lysholm.PLT <- nma("lysholm", "SMD", reference = "PLT"))
sink()

sink(file = "./lysholm.AT.txt")
print(lysholm.AT <- nma("lysholm", "SMD", reference = "AT"))
sink()

sink(file = "./lysholm.TA.txt")
print(lysholm.TA <- nma("lysholm", "SMD", reference = "TA"))
sink()
```

3. **Tegner**

```R

sink(file = "./tegner.BPTB.txt")
print(tegner.BPTB <- nma("tegner", "SMD", reference = "BPTB"))
sink()

sink(file = "./tegner.HT.txt")
print(tegner.HT <- nma("tegner", "SMD", reference = "HT"))
sink()

sink(file = "./tegner.QT.txt")
print(tegner.QT <- nma("tegner", "SMD", reference = "QT"))
sink()

sink(file = "./tegner.PLT.txt")
print(tegner.PLT <- nma("tegner", "SMD", reference = "PLT"))
sink()

#sink(file = "./tegner.AT.txt")
#print(tegner.AT <- nma("tegner", "SMD", reference = "AT"))
#sink()

sink(file = "./tegner.TA.txt")
print(tegner.TA <- nma("tegner", "SMD", reference = "TA"))
sink()
```

4. **Instrumented laxity**

```R

sink(file = "./instrumented_laxity.BPTB.txt")
print(instrumented_laxity.BPTB <- nma("instrumented_laxity", "SMD", reference = "BPTB"))
sink()

sink(file = "./instrumented_laxity.HT.txt")
print(instrumented_laxity.HT <- nma("instrumented_laxity", "SMD", reference = "HT"))
sink()

sink(file = "./instrumented_laxity.QT.txt")
print(instrumented_laxity.QT <- nma("instrumented_laxity", "SMD", reference = "QT"))
sink()

sink(file = "./instrumented_laxity.PLT.txt")
print(instrumented_laxity.PLT <- nma("instrumented_laxity", "SMD", reference = "PLT"))
sink()

#sink(file = "./instrumented_laxity.AT.txt")
#print(instrumented_laxity.AT <- nma("instrumented_laxity", "SMD", reference = "AT"))
#sink()

sink(file = "./instrumented_laxity.TA.txt")
print(instrumented_laxity.TA <- nma("instrumented_laxity", "SMD", reference = "TA"))
sink()

```

<!-- must figure out how to use lapply ***using two arguments as vector lists***--->

**Function applied** to a vector list of dataset.

```R

continuous = c("ikdc_subjective", "lysholm", "tegner", "instrumented_laxity")
lapply(continuous, sm = "SMD", network_meta_analysis)
```

```R
dichotomous = c("pivot_shift_1", "pivot_shift_2", "lachman_1", "lachman_2", "graft_failure")
lapply(dichotomous, sm = "OR", network_meta_analysis)
```
