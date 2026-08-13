Custom R function for running **network meta-analysis**

```R
network_meta_analysis <- function(df) {
  
  data_path <- paste0("G:/My Drive/github/d-wkim/network_meta-analysis/3_meta-analysis/R/analysis/sheets_for_analysis/",df, ".csv")
  
  data <- read_csv(data_path)
  
  m.netmeta <- netmeta(
		TE = smd, 
		seTE = se,
		treat1 = treat1,
		treat2 = treat2,
		studlab = study,
		data = data,
		sm = "SMD",
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
	long.labels = c("Bone-patellar tendon-bone", "Hamstring", "Peroneus longus", "Quadriceps", "Tibialis anterior")

	svg = paste0("G:/My Drive/github/d-wkim/network_meta-analysis/3_meta-analysis/R/plots/network_graph/", df, ".svg")

	svg(file = svg, height = 5, width = 5)

	network_graph = netgraph(m.netmeta, labels = labels, dim = "2d", scale = 1.1)
	print(network_graph)
	dev.off()
	
  return(m.netmeta)	
	
}
```

**Function applied** to a vector list of dataset.

```R
df = c("ikdc_subjective", 
       "lysholm", 
       "tegner", 
       "instrumented_laxity", 
       "pivot_shift_1", 
       "pivot_shift_2", 
       "lachman_1", 
       "lachman_2", 
       "graft_failure"
       )


lapply(df, DKIM_NMA)
```
