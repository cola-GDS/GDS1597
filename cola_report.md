cola Report for GDS1597
==================

**Date**: 2019-12-25 20:17:11 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 19411    51
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:kmeans](#SD-kmeans)     |          2| 1.000|           0.982|       0.992|** |           |
|[SD:NMF](#SD-NMF)           |          3| 1.000|           0.978|       0.990|** |2          |
|[CV:mclust](#CV-mclust)     |          2| 1.000|           0.983|       0.993|** |           |
|[MAD:hclust](#MAD-hclust)   |          2| 1.000|           0.979|       0.990|** |           |
|[MAD:mclust](#MAD-mclust)   |          2| 1.000|           0.949|       0.981|** |           |
|[ATC:hclust](#ATC-hclust)   |          3| 1.000|           1.000|       1.000|** |           |
|[ATC:kmeans](#ATC-kmeans)   |          2| 1.000|           0.974|       0.990|** |           |
|[ATC:pam](#ATC-pam)         |          2| 1.000|           0.991|       0.996|** |           |
|[SD:pam](#SD-pam)           |          6| 0.995|           0.952|       0.975|** |2,4,5      |
|[MAD:skmeans](#MAD-skmeans) |          3| 0.943|           0.950|       0.975|*  |2          |
|[MAD:NMF](#MAD-NMF)         |          3| 0.937|           0.908|       0.961|*  |2          |
|[MAD:pam](#MAD-pam)         |          6| 0.925|           0.903|       0.956|*  |5          |
|[ATC:skmeans](#ATC-skmeans) |          3| 0.918|           0.936|       0.956|*  |2          |
|[SD:skmeans](#SD-skmeans)   |          3| 0.918|           0.932|       0.972|*  |2          |
|[SD:hclust](#SD-hclust)     |          5| 0.903|           0.938|       0.971|*  |2          |
|[CV:pam](#CV-pam)           |          3| 0.888|           0.928|       0.965|   |           |
|[CV:skmeans](#CV-skmeans)   |          3| 0.880|           0.932|       0.970|   |           |
|[ATC:NMF](#ATC-NMF)         |          2| 0.879|           0.891|       0.957|   |           |
|[ATC:mclust](#ATC-mclust)   |          5| 0.871|           0.899|       0.956|   |           |
|[SD:mclust](#SD-mclust)     |          2| 0.824|           0.882|       0.938|   |           |
|[MAD:kmeans](#MAD-kmeans)   |          5| 0.768|           0.813|       0.852|   |           |
|[CV:NMF](#CV-NMF)           |          2| 0.728|           0.845|       0.938|   |           |
|[CV:kmeans](#CV-kmeans)     |          2| 0.437|           0.794|       0.886|   |           |
|[CV:hclust](#CV-hclust)     |          3| 0.400|           0.584|       0.765|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 1.000           0.978       0.990          0.415 0.594   0.594
#&gt; CV:NMF      2 0.728           0.845       0.938          0.465 0.547   0.547
#&gt; MAD:NMF     2 0.918           0.908       0.964          0.440 0.561   0.561
#&gt; ATC:NMF     2 0.879           0.891       0.957          0.441 0.547   0.547
#&gt; SD:skmeans  2 0.957           0.965       0.982          0.497 0.506   0.506
#&gt; CV:skmeans  2 0.841           0.929       0.970          0.508 0.492   0.492
#&gt; MAD:skmeans 2 0.918           0.924       0.967          0.500 0.506   0.506
#&gt; ATC:skmeans 2 1.000           0.991       0.996          0.488 0.514   0.514
#&gt; SD:mclust   2 0.824           0.882       0.938          0.445 0.534   0.534
#&gt; CV:mclust   2 1.000           0.983       0.993          0.394 0.613   0.613
#&gt; MAD:mclust  2 1.000           0.949       0.981          0.465 0.534   0.534
#&gt; ATC:mclust  2 0.680           0.868       0.926          0.471 0.534   0.534
#&gt; SD:kmeans   2 1.000           0.982       0.992          0.402 0.594   0.594
#&gt; CV:kmeans   2 0.437           0.794       0.886          0.468 0.500   0.500
#&gt; MAD:kmeans  2 0.849           0.933       0.967          0.429 0.594   0.594
#&gt; ATC:kmeans  2 1.000           0.974       0.990          0.479 0.523   0.523
#&gt; SD:pam      2 0.958           0.926       0.972          0.381 0.613   0.613
#&gt; CV:pam      2 0.731           0.800       0.924          0.361 0.633   0.633
#&gt; MAD:pam     2 0.841           0.872       0.949          0.429 0.561   0.561
#&gt; ATC:pam     2 1.000           0.991       0.996          0.479 0.523   0.523
#&gt; SD:hclust   2 1.000           0.995       0.998          0.390 0.613   0.613
#&gt; CV:hclust   2 0.361           0.661       0.827          0.370 0.730   0.730
#&gt; MAD:hclust  2 1.000           0.979       0.990          0.422 0.576   0.576
#&gt; ATC:hclust  2 0.758           0.957       0.977          0.405 0.613   0.613
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 1.000           0.978       0.990          0.396 0.749   0.603
#&gt; CV:NMF      3 0.570           0.765       0.871          0.343 0.742   0.564
#&gt; MAD:NMF     3 0.937           0.908       0.961          0.357 0.772   0.618
#&gt; ATC:NMF     3 0.713           0.775       0.867          0.202 0.881   0.787
#&gt; SD:skmeans  3 0.918           0.932       0.972          0.313 0.699   0.477
#&gt; CV:skmeans  3 0.880           0.932       0.970          0.308 0.737   0.517
#&gt; MAD:skmeans 3 0.943           0.950       0.975          0.305 0.733   0.523
#&gt; ATC:skmeans 3 0.918           0.936       0.956          0.172 0.900   0.807
#&gt; SD:mclust   3 0.596           0.551       0.812          0.325 0.721   0.522
#&gt; CV:mclust   3 0.624           0.815       0.893          0.626 0.708   0.531
#&gt; MAD:mclust  3 0.746           0.900       0.925          0.359 0.654   0.434
#&gt; ATC:mclust  3 0.648           0.849       0.883          0.349 0.767   0.576
#&gt; SD:kmeans   3 0.522           0.652       0.844          0.473 0.795   0.668
#&gt; CV:kmeans   3 0.662           0.781       0.883          0.349 0.695   0.463
#&gt; MAD:kmeans  3 0.676           0.780       0.882          0.451 0.757   0.596
#&gt; ATC:kmeans  3 0.867           0.831       0.929          0.302 0.809   0.651
#&gt; SD:pam      3 0.848           0.867       0.950          0.683 0.663   0.484
#&gt; CV:pam      3 0.888           0.928       0.965          0.793 0.645   0.473
#&gt; MAD:pam     3 0.674           0.833       0.928          0.509 0.707   0.514
#&gt; ATC:pam     3 0.821           0.938       0.968          0.374 0.706   0.488
#&gt; SD:hclust   3 0.687           0.926       0.915          0.207 0.967   0.946
#&gt; CV:hclust   3 0.400           0.584       0.765          0.563 0.802   0.729
#&gt; MAD:hclust  3 0.750           0.833       0.910          0.480 0.784   0.626
#&gt; ATC:hclust  3 1.000           1.000       1.000          0.380 0.830   0.722
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.830           0.855       0.923         0.2735 0.788   0.522
#&gt; CV:NMF      4 0.539           0.499       0.748         0.1233 0.940   0.844
#&gt; MAD:NMF     4 0.855           0.862       0.936         0.2510 0.807   0.549
#&gt; ATC:NMF     4 0.596           0.657       0.799         0.1870 0.874   0.738
#&gt; SD:skmeans  4 0.828           0.823       0.907         0.1173 0.897   0.711
#&gt; CV:skmeans  4 0.718           0.610       0.823         0.1139 0.976   0.929
#&gt; MAD:skmeans 4 0.794           0.822       0.860         0.1237 0.882   0.675
#&gt; ATC:skmeans 4 0.802           0.901       0.929         0.1754 0.874   0.702
#&gt; SD:mclust   4 0.586           0.681       0.806         0.2060 0.839   0.596
#&gt; CV:mclust   4 0.801           0.859       0.901         0.0926 0.962   0.890
#&gt; MAD:mclust  4 0.866           0.907       0.954         0.1468 0.939   0.821
#&gt; ATC:mclust  4 0.717           0.757       0.883         0.0983 0.835   0.574
#&gt; SD:kmeans   4 0.615           0.733       0.798         0.1664 0.740   0.472
#&gt; CV:kmeans   4 0.579           0.611       0.770         0.1149 0.976   0.929
#&gt; MAD:kmeans  4 0.629           0.621       0.777         0.1514 0.817   0.553
#&gt; ATC:kmeans  4 0.723           0.867       0.867         0.1586 0.805   0.530
#&gt; SD:pam      4 0.972           0.954       0.980         0.1122 0.889   0.708
#&gt; CV:pam      4 0.860           0.920       0.938         0.0707 0.955   0.874
#&gt; MAD:pam     4 0.761           0.824       0.889         0.0950 0.865   0.650
#&gt; ATC:pam     4 0.838           0.905       0.919         0.0801 0.967   0.901
#&gt; SD:hclust   4 0.889           0.938       0.971         0.4410 0.745   0.560
#&gt; CV:hclust   4 0.551           0.641       0.745         0.1180 0.869   0.765
#&gt; MAD:hclust  4 0.725           0.821       0.907         0.0818 0.956   0.878
#&gt; ATC:hclust  4 0.793           0.935       0.916         0.1170 0.967   0.926
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.814           0.779       0.893         0.0416 0.940   0.779
#&gt; CV:NMF      5 0.668           0.678       0.825         0.1012 0.829   0.527
#&gt; MAD:NMF     5 0.777           0.691       0.871         0.0327 0.890   0.641
#&gt; ATC:NMF     5 0.566           0.598       0.768         0.1163 0.894   0.735
#&gt; SD:skmeans  5 0.749           0.747       0.821         0.0618 0.978   0.917
#&gt; CV:skmeans  5 0.682           0.487       0.707         0.0582 0.786   0.412
#&gt; MAD:skmeans 5 0.735           0.715       0.831         0.0579 0.960   0.850
#&gt; ATC:skmeans 5 0.778           0.810       0.890         0.0584 0.982   0.940
#&gt; SD:mclust   5 0.662           0.624       0.769         0.0810 0.854   0.546
#&gt; CV:mclust   5 0.675           0.795       0.862         0.0458 0.952   0.850
#&gt; MAD:mclust  5 0.771           0.574       0.820         0.0906 0.929   0.749
#&gt; ATC:mclust  5 0.871           0.899       0.956         0.0620 0.944   0.806
#&gt; SD:kmeans   5 0.839           0.882       0.900         0.0828 0.978   0.924
#&gt; CV:kmeans   5 0.644           0.667       0.764         0.0807 0.933   0.788
#&gt; MAD:kmeans  5 0.768           0.813       0.852         0.0727 0.964   0.869
#&gt; ATC:kmeans  5 0.865           0.852       0.865         0.0728 0.972   0.889
#&gt; SD:pam      5 1.000           0.967       0.986         0.0507 0.955   0.846
#&gt; CV:pam      5 0.705           0.767       0.876         0.0617 0.835   0.569
#&gt; MAD:pam     5 0.944           0.958       0.978         0.0553 0.965   0.879
#&gt; ATC:pam     5 0.873           0.783       0.889         0.0783 0.882   0.631
#&gt; SD:hclust   5 0.903           0.938       0.971         0.0946 0.934   0.797
#&gt; CV:hclust   5 0.571           0.671       0.760         0.0390 0.991   0.979
#&gt; MAD:hclust  5 0.698           0.800       0.874         0.1189 0.934   0.792
#&gt; ATC:hclust  5 0.780           0.826       0.873         0.1731 0.813   0.544
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.708           0.666       0.832         0.0343 0.944   0.784
#&gt; CV:NMF      6 0.702           0.529       0.779         0.0440 0.951   0.799
#&gt; MAD:NMF     6 0.694           0.658       0.837         0.0305 0.975   0.900
#&gt; ATC:NMF     6 0.573           0.522       0.738         0.0529 0.921   0.761
#&gt; SD:skmeans  6 0.777           0.751       0.853         0.0555 0.878   0.552
#&gt; CV:skmeans  6 0.756           0.699       0.835         0.0483 0.876   0.511
#&gt; MAD:skmeans 6 0.767           0.732       0.849         0.0553 0.896   0.593
#&gt; ATC:skmeans 6 0.809           0.843       0.915         0.0498 0.947   0.818
#&gt; SD:mclust   6 0.775           0.808       0.862         0.0529 0.893   0.574
#&gt; CV:mclust   6 0.748           0.738       0.839         0.0796 0.909   0.688
#&gt; MAD:mclust  6 0.818           0.756       0.825         0.0405 0.896   0.574
#&gt; ATC:mclust  6 0.855           0.833       0.880         0.0609 0.917   0.683
#&gt; SD:kmeans   6 0.799           0.736       0.822         0.0757 0.933   0.749
#&gt; CV:kmeans   6 0.684           0.592       0.727         0.0554 0.875   0.558
#&gt; MAD:kmeans  6 0.773           0.763       0.839         0.0568 0.917   0.677
#&gt; ATC:kmeans  6 0.846           0.818       0.864         0.0450 0.955   0.801
#&gt; SD:pam      6 0.995           0.952       0.975         0.0980 0.870   0.535
#&gt; CV:pam      6 0.864           0.874       0.931         0.1038 0.801   0.409
#&gt; MAD:pam     6 0.925           0.903       0.956         0.1023 0.870   0.535
#&gt; ATC:pam     6 0.868           0.813       0.901         0.0569 0.953   0.794
#&gt; SD:hclust   6 0.871           0.915       0.929         0.0357 0.969   0.879
#&gt; CV:hclust   6 0.761           0.813       0.868         0.1635 0.787   0.518
#&gt; MAD:hclust  6 0.738           0.750       0.844         0.0361 0.988   0.953
#&gt; ATC:hclust  6 0.761           0.843       0.847         0.0223 0.957   0.813
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n individual(p) k
#&gt; SD:NMF      51       0.04614 2
#&gt; CV:NMF      47       0.11853 2
#&gt; MAD:NMF     48       0.07959 2
#&gt; ATC:NMF     47       0.09479 2
#&gt; SD:skmeans  51       0.07878 2
#&gt; CV:skmeans  50       0.04377 2
#&gt; MAD:skmeans 48       0.03827 2
#&gt; ATC:skmeans 51       0.02014 2
#&gt; SD:mclust   49       0.01812 2
#&gt; CV:mclust   50       0.01498 2
#&gt; MAD:mclust  49       0.02556 2
#&gt; ATC:mclust  47       0.00427 2
#&gt; SD:kmeans   51       0.04614 2
#&gt; CV:kmeans   49       0.06203 2
#&gt; MAD:kmeans  50       0.05554 2
#&gt; ATC:kmeans  50       0.02722 2
#&gt; SD:pam      48       0.02976 2
#&gt; CV:pam      43       0.01685 2
#&gt; MAD:pam     47       0.04562 2
#&gt; ATC:pam     51       0.04827 2
#&gt; SD:hclust   51       0.05676 2
#&gt; CV:hclust   41       0.15730 2
#&gt; MAD:hclust  51       0.13676 2
#&gt; ATC:hclust  51       0.05676 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n individual(p) k
#&gt; SD:NMF      51      0.008714 3
#&gt; CV:NMF      47      0.001895 3
#&gt; MAD:NMF     49      0.008434 3
#&gt; ATC:NMF     46      0.002682 3
#&gt; SD:skmeans  50      0.000260 3
#&gt; CV:skmeans  50      0.003367 3
#&gt; MAD:skmeans 51      0.000385 3
#&gt; ATC:skmeans 50      0.019085 3
#&gt; SD:mclust   31      0.004521 3
#&gt; CV:mclust   47      0.002891 3
#&gt; MAD:mclust  51      0.000095 3
#&gt; ATC:mclust  49      0.003068 3
#&gt; SD:kmeans   38      0.007908 3
#&gt; CV:kmeans   43      0.002090 3
#&gt; MAD:kmeans  49      0.032939 3
#&gt; ATC:kmeans  46      0.031872 3
#&gt; SD:pam      46      0.008049 3
#&gt; CV:pam      51      0.003902 3
#&gt; MAD:pam     46      0.003165 3
#&gt; ATC:pam     50      0.068536 3
#&gt; SD:hclust   51      0.006531 3
#&gt; CV:hclust   34      0.002249 3
#&gt; MAD:hclust  48      0.033987 3
#&gt; ATC:hclust  51      0.014136 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n individual(p) k
#&gt; SD:NMF      48      0.000723 4
#&gt; CV:NMF      35      0.000419 4
#&gt; MAD:NMF     47      0.000522 4
#&gt; ATC:NMF     36      0.068195 4
#&gt; SD:skmeans  47      0.000834 4
#&gt; CV:skmeans  41      0.003736 4
#&gt; MAD:skmeans 50      0.001025 4
#&gt; ATC:skmeans 50      0.002094 4
#&gt; SD:mclust   43      0.000760 4
#&gt; CV:mclust   48      0.003170 4
#&gt; MAD:mclust  51      0.000448 4
#&gt; ATC:mclust  40      0.012193 4
#&gt; SD:kmeans   46      0.001667 4
#&gt; CV:kmeans   39      0.008902 4
#&gt; MAD:kmeans  37      0.034876 4
#&gt; ATC:kmeans  50      0.031703 4
#&gt; SD:pam      51      0.013915 4
#&gt; CV:pam      51      0.001096 4
#&gt; MAD:pam     50      0.017650 4
#&gt; ATC:pam     50      0.005452 4
#&gt; SD:hclust   51      0.003686 4
#&gt; CV:hclust   43      0.000987 4
#&gt; MAD:hclust  48      0.004140 4
#&gt; ATC:hclust  51      0.001835 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n individual(p) k
#&gt; SD:NMF      46      0.002081 5
#&gt; CV:NMF      38      0.000213 5
#&gt; MAD:NMF     41      0.001165 5
#&gt; ATC:NMF     39      0.014891 5
#&gt; SD:skmeans  47      0.000199 5
#&gt; CV:skmeans  26      0.037852 5
#&gt; MAD:skmeans 45      0.002264 5
#&gt; ATC:skmeans 44      0.006353 5
#&gt; SD:mclust   40      0.005273 5
#&gt; CV:mclust   49      0.004350 5
#&gt; MAD:mclust  34      0.016251 5
#&gt; ATC:mclust  47      0.000492 5
#&gt; SD:kmeans   51      0.000942 5
#&gt; CV:kmeans   44      0.000842 5
#&gt; MAD:kmeans  49      0.000212 5
#&gt; ATC:kmeans  50      0.013392 5
#&gt; SD:pam      51      0.003717 5
#&gt; CV:pam      47      0.003156 5
#&gt; MAD:pam     51      0.003717 5
#&gt; ATC:pam     43      0.003428 5
#&gt; SD:hclust   51      0.000733 5
#&gt; CV:hclust   44      0.000811 5
#&gt; MAD:hclust  46      0.000429 5
#&gt; ATC:hclust  47      0.001404 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n individual(p) k
#&gt; SD:NMF      42      0.003813 6
#&gt; CV:NMF      35      0.000478 6
#&gt; MAD:NMF     41      0.000245 6
#&gt; ATC:NMF     30      0.000921 6
#&gt; SD:skmeans  45      0.000932 6
#&gt; CV:skmeans  44      0.000724 6
#&gt; MAD:skmeans 45      0.000476 6
#&gt; ATC:skmeans 47      0.004663 6
#&gt; SD:mclust   49      0.000791 6
#&gt; CV:mclust   44      0.031893 6
#&gt; MAD:mclust  47      0.000245 6
#&gt; ATC:mclust  47      0.002607 6
#&gt; SD:kmeans   44      0.001207 6
#&gt; CV:kmeans   31      0.000702 6
#&gt; MAD:kmeans  47      0.000536 6
#&gt; ATC:kmeans  49      0.001691 6
#&gt; SD:pam      50      0.001075 6
#&gt; CV:pam      50      0.003227 6
#&gt; MAD:pam     49      0.000743 6
#&gt; ATC:pam     48      0.000235 6
#&gt; SD:hclust   51      0.000234 6
#&gt; CV:hclust   48      0.005963 6
#&gt; MAD:hclust  44      0.001062 6
#&gt; ATC:hclust  49      0.000480 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.995       0.998         0.3902 0.613   0.613
#> 3 3 0.687           0.926       0.915         0.2068 0.967   0.946
#> 4 4 0.889           0.938       0.971         0.4410 0.745   0.560
#> 5 5 0.903           0.938       0.971         0.0946 0.934   0.797
#> 6 6 0.871           0.915       0.929         0.0357 0.969   0.879
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.997 1.000 0.000
#&gt; GSM38713     1   0.000      0.997 1.000 0.000
#&gt; GSM38714     1   0.000      0.997 1.000 0.000
#&gt; GSM38715     1   0.000      0.997 1.000 0.000
#&gt; GSM38716     1   0.000      0.997 1.000 0.000
#&gt; GSM38717     1   0.000      0.997 1.000 0.000
#&gt; GSM38718     1   0.000      0.997 1.000 0.000
#&gt; GSM38719     1   0.000      0.997 1.000 0.000
#&gt; GSM38720     1   0.000      0.997 1.000 0.000
#&gt; GSM38721     1   0.000      0.997 1.000 0.000
#&gt; GSM38722     1   0.000      0.997 1.000 0.000
#&gt; GSM38723     1   0.000      0.997 1.000 0.000
#&gt; GSM38724     1   0.000      0.997 1.000 0.000
#&gt; GSM38725     1   0.000      0.997 1.000 0.000
#&gt; GSM38726     1   0.000      0.997 1.000 0.000
#&gt; GSM38727     1   0.000      0.997 1.000 0.000
#&gt; GSM38728     1   0.000      0.997 1.000 0.000
#&gt; GSM38729     1   0.000      0.997 1.000 0.000
#&gt; GSM38730     1   0.000      0.997 1.000 0.000
#&gt; GSM38731     1   0.000      0.997 1.000 0.000
#&gt; GSM38732     1   0.358      0.929 0.932 0.068
#&gt; GSM38733     1   0.000      0.997 1.000 0.000
#&gt; GSM38734     2   0.000      1.000 0.000 1.000
#&gt; GSM38735     1   0.000      0.997 1.000 0.000
#&gt; GSM38736     2   0.000      1.000 0.000 1.000
#&gt; GSM38737     2   0.000      1.000 0.000 1.000
#&gt; GSM38738     1   0.295      0.946 0.948 0.052
#&gt; GSM38739     1   0.000      0.997 1.000 0.000
#&gt; GSM38740     1   0.000      0.997 1.000 0.000
#&gt; GSM38741     1   0.000      0.997 1.000 0.000
#&gt; GSM38742     2   0.000      1.000 0.000 1.000
#&gt; GSM38743     2   0.000      1.000 0.000 1.000
#&gt; GSM38744     1   0.000      0.997 1.000 0.000
#&gt; GSM38745     1   0.000      0.997 1.000 0.000
#&gt; GSM38746     1   0.000      0.997 1.000 0.000
#&gt; GSM38747     1   0.000      0.997 1.000 0.000
#&gt; GSM38748     2   0.000      1.000 0.000 1.000
#&gt; GSM38749     1   0.000      0.997 1.000 0.000
#&gt; GSM38750     1   0.000      0.997 1.000 0.000
#&gt; GSM38751     1   0.000      0.997 1.000 0.000
#&gt; GSM38752     2   0.000      1.000 0.000 1.000
#&gt; GSM38753     2   0.000      1.000 0.000 1.000
#&gt; GSM38754     2   0.000      1.000 0.000 1.000
#&gt; GSM38755     1   0.000      0.997 1.000 0.000
#&gt; GSM38756     2   0.000      1.000 0.000 1.000
#&gt; GSM38757     1   0.000      0.997 1.000 0.000
#&gt; GSM38758     2   0.000      1.000 0.000 1.000
#&gt; GSM38759     1   0.000      0.997 1.000 0.000
#&gt; GSM38760     1   0.000      0.997 1.000 0.000
#&gt; GSM38761     2   0.000      1.000 0.000 1.000
#&gt; GSM38762     2   0.000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38721     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38722     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38724     1  0.4555      0.866 0.800 0.200 0.000
#&gt; GSM38725     1  0.2959      0.904 0.900 0.100 0.000
#&gt; GSM38726     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38728     1  0.4796      0.854 0.780 0.220 0.000
#&gt; GSM38729     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38732     1  0.6810      0.801 0.720 0.212 0.068
#&gt; GSM38733     1  0.0237      0.925 0.996 0.004 0.000
#&gt; GSM38734     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM38735     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38736     2  0.4796      1.000 0.000 0.780 0.220
#&gt; GSM38737     2  0.4796      1.000 0.000 0.780 0.220
#&gt; GSM38738     1  0.6446      0.817 0.736 0.212 0.052
#&gt; GSM38739     1  0.3116      0.902 0.892 0.108 0.000
#&gt; GSM38740     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38741     1  0.4796      0.854 0.780 0.220 0.000
#&gt; GSM38742     2  0.4796      1.000 0.000 0.780 0.220
#&gt; GSM38743     2  0.4796      1.000 0.000 0.780 0.220
#&gt; GSM38744     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38745     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM38746     1  0.4796      0.854 0.780 0.220 0.000
#&gt; GSM38747     1  0.4796      0.854 0.780 0.220 0.000
#&gt; GSM38748     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM38749     1  0.3116      0.902 0.892 0.108 0.000
#&gt; GSM38750     1  0.4555      0.866 0.800 0.200 0.000
#&gt; GSM38751     1  0.4555      0.866 0.800 0.200 0.000
#&gt; GSM38752     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM38753     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM38754     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM38755     1  0.3482      0.896 0.872 0.128 0.000
#&gt; GSM38756     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM38757     1  0.4555      0.866 0.800 0.200 0.000
#&gt; GSM38758     2  0.4796      1.000 0.000 0.780 0.220
#&gt; GSM38759     1  0.4796      0.854 0.780 0.220 0.000
#&gt; GSM38760     1  0.3482      0.896 0.872 0.128 0.000
#&gt; GSM38761     2  0.4796      1.000 0.000 0.780 0.220
#&gt; GSM38762     2  0.4796      1.000 0.000 0.780 0.220
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4
#&gt; GSM38712     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38713     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38714     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38715     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38716     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38717     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38718     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38719     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38720     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38721     1  0.2704      0.858 0.876  0 0.124 0.000
#&gt; GSM38722     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38723     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38724     3  0.0707      0.944 0.020  0 0.980 0.000
#&gt; GSM38725     1  0.2345      0.873 0.900  0 0.100 0.000
#&gt; GSM38726     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38727     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38728     3  0.0000      0.944 0.000  0 1.000 0.000
#&gt; GSM38729     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38730     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38731     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38732     3  0.1792      0.905 0.000  0 0.932 0.068
#&gt; GSM38733     1  0.2760      0.854 0.872  0 0.128 0.000
#&gt; GSM38734     4  0.0000      1.000 0.000  0 0.000 1.000
#&gt; GSM38735     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM38738     3  0.1474      0.918 0.000  0 0.948 0.052
#&gt; GSM38739     1  0.4356      0.614 0.708  0 0.292 0.000
#&gt; GSM38740     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38741     3  0.0000      0.944 0.000  0 1.000 0.000
#&gt; GSM38742     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM38744     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38745     1  0.0000      0.958 1.000  0 0.000 0.000
#&gt; GSM38746     3  0.0000      0.944 0.000  0 1.000 0.000
#&gt; GSM38747     3  0.0000      0.944 0.000  0 1.000 0.000
#&gt; GSM38748     4  0.0000      1.000 0.000  0 0.000 1.000
#&gt; GSM38749     1  0.4356      0.614 0.708  0 0.292 0.000
#&gt; GSM38750     3  0.0707      0.944 0.020  0 0.980 0.000
#&gt; GSM38751     3  0.0707      0.944 0.020  0 0.980 0.000
#&gt; GSM38752     4  0.0000      1.000 0.000  0 0.000 1.000
#&gt; GSM38753     4  0.0000      1.000 0.000  0 0.000 1.000
#&gt; GSM38754     4  0.0000      1.000 0.000  0 0.000 1.000
#&gt; GSM38755     3  0.2704      0.839 0.124  0 0.876 0.000
#&gt; GSM38756     4  0.0000      1.000 0.000  0 0.000 1.000
#&gt; GSM38757     3  0.0707      0.944 0.020  0 0.980 0.000
#&gt; GSM38758     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM38759     3  0.0000      0.944 0.000  0 1.000 0.000
#&gt; GSM38760     3  0.3688      0.732 0.208  0 0.792 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4 p5
#&gt; GSM38712     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38713     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38714     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38715     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38716     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38717     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38718     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38719     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38720     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38721     1  0.2329      0.851 0.876  0 0.124 0.000  0
#&gt; GSM38722     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38723     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38724     3  0.0609      0.944 0.020  0 0.980 0.000  0
#&gt; GSM38725     1  0.2020      0.865 0.900  0 0.100 0.000  0
#&gt; GSM38726     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38727     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38728     3  0.0000      0.944 0.000  0 1.000 0.000  0
#&gt; GSM38729     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38730     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38731     1  0.0000      0.947 1.000  0 0.000 0.000  0
#&gt; GSM38732     3  0.1544      0.905 0.000  0 0.932 0.068  0
#&gt; GSM38733     1  0.2377      0.848 0.872  0 0.128 0.000  0
#&gt; GSM38734     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38735     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38736     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38737     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38738     3  0.1270      0.918 0.000  0 0.948 0.052  0
#&gt; GSM38739     1  0.3752      0.622 0.708  0 0.292 0.000  0
#&gt; GSM38740     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38741     3  0.0000      0.944 0.000  0 1.000 0.000  0
#&gt; GSM38742     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38743     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38744     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38745     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38746     3  0.0000      0.944 0.000  0 1.000 0.000  0
#&gt; GSM38747     3  0.0000      0.944 0.000  0 1.000 0.000  0
#&gt; GSM38748     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38749     1  0.3752      0.622 0.708  0 0.292 0.000  0
#&gt; GSM38750     3  0.0609      0.944 0.020  0 0.980 0.000  0
#&gt; GSM38751     3  0.0609      0.944 0.020  0 0.980 0.000  0
#&gt; GSM38752     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38753     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38754     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38755     3  0.2329      0.839 0.124  0 0.876 0.000  0
#&gt; GSM38756     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38757     3  0.0609      0.944 0.020  0 0.980 0.000  0
#&gt; GSM38758     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38759     3  0.0000      0.944 0.000  0 1.000 0.000  0
#&gt; GSM38760     3  0.3177      0.732 0.208  0 0.792 0.000  0
#&gt; GSM38761     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38762     2  0.0000      1.000 0.000  1 0.000 0.000  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4 p5    p6
#&gt; GSM38712     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38713     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38714     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38715     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38716     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38717     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38718     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38719     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38720     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38721     1  0.2092      0.845 0.876  0 0.124 0.000  0 0.000
#&gt; GSM38722     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38723     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38724     3  0.0291      0.875 0.004  0 0.992 0.000  0 0.004
#&gt; GSM38725     1  0.1814      0.866 0.900  0 0.100 0.000  0 0.000
#&gt; GSM38726     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38727     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38728     6  0.3482      0.978 0.000  0 0.316 0.000  0 0.684
#&gt; GSM38729     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38730     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38731     1  0.0000      0.948 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38732     3  0.1814      0.829 0.000  0 0.900 0.000  0 0.100
#&gt; GSM38733     1  0.2135      0.841 0.872  0 0.128 0.000  0 0.000
#&gt; GSM38734     4  0.2994      0.843 0.000  0 0.004 0.788  0 0.208
#&gt; GSM38735     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38738     3  0.1610      0.841 0.000  0 0.916 0.000  0 0.084
#&gt; GSM38739     1  0.3428      0.595 0.696  0 0.304 0.000  0 0.000
#&gt; GSM38740     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM38741     6  0.3482      0.978 0.000  0 0.316 0.000  0 0.684
#&gt; GSM38742     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38744     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM38745     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM38746     6  0.3482      0.967 0.000  0 0.316 0.000  0 0.684
#&gt; GSM38747     6  0.3482      0.967 0.000  0 0.316 0.000  0 0.684
#&gt; GSM38748     4  0.2730      0.849 0.000  0 0.000 0.808  0 0.192
#&gt; GSM38749     1  0.3428      0.595 0.696  0 0.304 0.000  0 0.000
#&gt; GSM38750     3  0.0291      0.875 0.004  0 0.992 0.000  0 0.004
#&gt; GSM38751     3  0.0291      0.875 0.004  0 0.992 0.000  0 0.004
#&gt; GSM38752     4  0.1765      0.888 0.000  0 0.000 0.904  0 0.096
#&gt; GSM38753     4  0.0000      0.904 0.000  0 0.000 1.000  0 0.000
#&gt; GSM38754     4  0.1765      0.888 0.000  0 0.000 0.904  0 0.096
#&gt; GSM38755     3  0.1910      0.777 0.108  0 0.892 0.000  0 0.000
#&gt; GSM38756     4  0.0000      0.904 0.000  0 0.000 1.000  0 0.000
#&gt; GSM38757     3  0.0291      0.875 0.004  0 0.992 0.000  0 0.004
#&gt; GSM38758     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38759     6  0.3482      0.978 0.000  0 0.316 0.000  0 0.684
#&gt; GSM38760     3  0.2730      0.654 0.192  0 0.808 0.000  0 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n individual(p) k
#> SD:hclust 51      0.056758 2
#> SD:hclust 51      0.006531 3
#> SD:hclust 51      0.003686 4
#> SD:hclust 51      0.000733 5
#> SD:hclust 51      0.000234 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.982       0.992         0.4016 0.594   0.594
#> 3 3 0.522           0.652       0.844         0.4734 0.795   0.668
#> 4 4 0.615           0.733       0.798         0.1664 0.740   0.472
#> 5 5 0.839           0.882       0.900         0.0828 0.978   0.924
#> 6 6 0.799           0.736       0.822         0.0757 0.933   0.749
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.997 1.000 0.000
#&gt; GSM38713     1  0.0000      0.997 1.000 0.000
#&gt; GSM38714     1  0.0000      0.997 1.000 0.000
#&gt; GSM38715     1  0.0000      0.997 1.000 0.000
#&gt; GSM38716     1  0.0000      0.997 1.000 0.000
#&gt; GSM38717     1  0.0000      0.997 1.000 0.000
#&gt; GSM38718     1  0.0000      0.997 1.000 0.000
#&gt; GSM38719     1  0.0000      0.997 1.000 0.000
#&gt; GSM38720     1  0.0000      0.997 1.000 0.000
#&gt; GSM38721     1  0.0000      0.997 1.000 0.000
#&gt; GSM38722     1  0.0000      0.997 1.000 0.000
#&gt; GSM38723     1  0.0000      0.997 1.000 0.000
#&gt; GSM38724     1  0.0000      0.997 1.000 0.000
#&gt; GSM38725     1  0.0000      0.997 1.000 0.000
#&gt; GSM38726     1  0.0000      0.997 1.000 0.000
#&gt; GSM38727     1  0.0000      0.997 1.000 0.000
#&gt; GSM38728     1  0.0672      0.994 0.992 0.008
#&gt; GSM38729     1  0.0000      0.997 1.000 0.000
#&gt; GSM38730     1  0.0000      0.997 1.000 0.000
#&gt; GSM38731     1  0.0000      0.997 1.000 0.000
#&gt; GSM38732     1  0.0672      0.994 0.992 0.008
#&gt; GSM38733     1  0.0000      0.997 1.000 0.000
#&gt; GSM38734     2  0.0000      0.975 0.000 1.000
#&gt; GSM38735     1  0.0672      0.994 0.992 0.008
#&gt; GSM38736     2  0.0000      0.975 0.000 1.000
#&gt; GSM38737     2  0.0000      0.975 0.000 1.000
#&gt; GSM38738     1  0.0672      0.994 0.992 0.008
#&gt; GSM38739     1  0.0000      0.997 1.000 0.000
#&gt; GSM38740     1  0.0000      0.997 1.000 0.000
#&gt; GSM38741     2  0.9044      0.526 0.320 0.680
#&gt; GSM38742     2  0.0000      0.975 0.000 1.000
#&gt; GSM38743     2  0.0000      0.975 0.000 1.000
#&gt; GSM38744     1  0.0000      0.997 1.000 0.000
#&gt; GSM38745     1  0.0672      0.994 0.992 0.008
#&gt; GSM38746     1  0.0672      0.994 0.992 0.008
#&gt; GSM38747     1  0.0672      0.994 0.992 0.008
#&gt; GSM38748     2  0.0000      0.975 0.000 1.000
#&gt; GSM38749     1  0.0000      0.997 1.000 0.000
#&gt; GSM38750     1  0.0672      0.994 0.992 0.008
#&gt; GSM38751     1  0.0672      0.994 0.992 0.008
#&gt; GSM38752     2  0.0000      0.975 0.000 1.000
#&gt; GSM38753     2  0.0000      0.975 0.000 1.000
#&gt; GSM38754     2  0.0000      0.975 0.000 1.000
#&gt; GSM38755     1  0.0000      0.997 1.000 0.000
#&gt; GSM38756     2  0.0000      0.975 0.000 1.000
#&gt; GSM38757     1  0.0672      0.994 0.992 0.008
#&gt; GSM38758     2  0.0000      0.975 0.000 1.000
#&gt; GSM38759     1  0.0672      0.994 0.992 0.008
#&gt; GSM38760     1  0.0000      0.997 1.000 0.000
#&gt; GSM38761     2  0.0000      0.975 0.000 1.000
#&gt; GSM38762     2  0.0000      0.975 0.000 1.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38713     1  0.2796      0.797 0.908 0.000 0.092
#&gt; GSM38714     1  0.2959      0.792 0.900 0.000 0.100
#&gt; GSM38715     1  0.2625      0.802 0.916 0.000 0.084
#&gt; GSM38716     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38721     1  0.2796      0.797 0.908 0.000 0.092
#&gt; GSM38722     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38724     1  0.6307      0.183 0.512 0.000 0.488
#&gt; GSM38725     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38728     3  0.4504      0.662 0.196 0.000 0.804
#&gt; GSM38729     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.834 1.000 0.000 0.000
#&gt; GSM38732     3  0.2878      0.678 0.096 0.000 0.904
#&gt; GSM38733     1  0.3267      0.784 0.884 0.000 0.116
#&gt; GSM38734     3  0.5465      0.383 0.000 0.288 0.712
#&gt; GSM38735     1  0.8880      0.421 0.564 0.268 0.168
#&gt; GSM38736     2  0.0000      0.851 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      0.851 0.000 1.000 0.000
#&gt; GSM38738     3  0.2878      0.678 0.096 0.000 0.904
#&gt; GSM38739     1  0.3412      0.774 0.876 0.000 0.124
#&gt; GSM38740     1  0.5791      0.711 0.784 0.048 0.168
#&gt; GSM38741     3  0.2384      0.656 0.056 0.008 0.936
#&gt; GSM38742     2  0.0237      0.851 0.000 0.996 0.004
#&gt; GSM38743     2  0.0000      0.851 0.000 1.000 0.000
#&gt; GSM38744     1  0.3765      0.767 0.888 0.028 0.084
#&gt; GSM38745     1  0.8853      0.428 0.568 0.264 0.168
#&gt; GSM38746     1  0.6307      0.183 0.512 0.000 0.488
#&gt; GSM38747     1  0.6307      0.183 0.512 0.000 0.488
#&gt; GSM38748     3  0.6026      0.155 0.000 0.376 0.624
#&gt; GSM38749     1  0.2165      0.814 0.936 0.000 0.064
#&gt; GSM38750     3  0.5363      0.563 0.276 0.000 0.724
#&gt; GSM38751     3  0.5397      0.555 0.280 0.000 0.720
#&gt; GSM38752     3  0.5529      0.370 0.000 0.296 0.704
#&gt; GSM38753     2  0.6309      0.156 0.000 0.504 0.496
#&gt; GSM38754     3  0.5529      0.370 0.000 0.296 0.704
#&gt; GSM38755     1  0.6291      0.216 0.532 0.000 0.468
#&gt; GSM38756     2  0.6309      0.156 0.000 0.504 0.496
#&gt; GSM38757     3  0.5363      0.563 0.276 0.000 0.724
#&gt; GSM38758     2  0.1163      0.837 0.000 0.972 0.028
#&gt; GSM38759     1  0.6305      0.195 0.516 0.000 0.484
#&gt; GSM38760     1  0.2165      0.814 0.936 0.000 0.064
#&gt; GSM38761     2  0.0237      0.851 0.000 0.996 0.004
#&gt; GSM38762     2  0.0000      0.851 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.3102      0.849 0.872 0.004 0.116 0.008
#&gt; GSM38714     1  0.3632      0.807 0.832 0.004 0.156 0.008
#&gt; GSM38715     1  0.1994      0.900 0.936 0.004 0.052 0.008
#&gt; GSM38716     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0188      0.932 0.996 0.000 0.000 0.004
#&gt; GSM38718     1  0.0336      0.930 0.992 0.000 0.000 0.008
#&gt; GSM38719     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.3102      0.849 0.872 0.004 0.116 0.008
#&gt; GSM38722     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.2944      0.805 0.128 0.000 0.868 0.004
#&gt; GSM38725     1  0.0188      0.932 0.996 0.000 0.000 0.004
#&gt; GSM38726     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38728     3  0.2565      0.778 0.032 0.000 0.912 0.056
#&gt; GSM38729     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.3182      0.680 0.004 0.004 0.860 0.132
#&gt; GSM38733     1  0.3907      0.774 0.808 0.004 0.180 0.008
#&gt; GSM38734     4  0.4948      0.638 0.000 0.000 0.440 0.560
#&gt; GSM38735     2  0.8521      0.300 0.212 0.536 0.148 0.104
#&gt; GSM38736     2  0.4730      0.559 0.000 0.636 0.000 0.364
#&gt; GSM38737     2  0.4730      0.559 0.000 0.636 0.000 0.364
#&gt; GSM38738     3  0.3128      0.686 0.004 0.004 0.864 0.128
#&gt; GSM38739     1  0.3727      0.797 0.824 0.008 0.164 0.004
#&gt; GSM38740     2  0.8996      0.223 0.324 0.424 0.148 0.104
#&gt; GSM38741     3  0.2868      0.671 0.000 0.000 0.864 0.136
#&gt; GSM38742     2  0.4761      0.555 0.000 0.628 0.000 0.372
#&gt; GSM38743     2  0.4730      0.559 0.000 0.636 0.000 0.364
#&gt; GSM38744     2  0.8923      0.206 0.336 0.424 0.136 0.104
#&gt; GSM38745     2  0.8547      0.299 0.216 0.532 0.148 0.104
#&gt; GSM38746     3  0.2859      0.809 0.112 0.008 0.880 0.000
#&gt; GSM38747     3  0.2530      0.812 0.112 0.000 0.888 0.000
#&gt; GSM38748     4  0.4164      0.700 0.000 0.000 0.264 0.736
#&gt; GSM38749     1  0.3355      0.810 0.836 0.000 0.160 0.004
#&gt; GSM38750     3  0.1722      0.819 0.048 0.008 0.944 0.000
#&gt; GSM38751     3  0.1722      0.819 0.048 0.008 0.944 0.000
#&gt; GSM38752     4  0.4948      0.638 0.000 0.000 0.440 0.560
#&gt; GSM38753     4  0.2737      0.561 0.000 0.008 0.104 0.888
#&gt; GSM38754     4  0.4948      0.638 0.000 0.000 0.440 0.560
#&gt; GSM38755     3  0.4772      0.618 0.244 0.012 0.736 0.008
#&gt; GSM38756     4  0.2737      0.561 0.000 0.008 0.104 0.888
#&gt; GSM38757     3  0.1975      0.817 0.048 0.000 0.936 0.016
#&gt; GSM38758     2  0.4925      0.487 0.000 0.572 0.000 0.428
#&gt; GSM38759     3  0.2944      0.805 0.128 0.000 0.868 0.004
#&gt; GSM38760     1  0.2921      0.829 0.860 0.000 0.140 0.000
#&gt; GSM38761     2  0.4761      0.555 0.000 0.628 0.000 0.372
#&gt; GSM38762     2  0.4730      0.559 0.000 0.636 0.000 0.364
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0451      0.880 0.988 0.000 0.000 0.004 0.008
#&gt; GSM38713     1  0.4425      0.814 0.784 0.000 0.020 0.132 0.064
#&gt; GSM38714     1  0.5106      0.785 0.748 0.000 0.056 0.132 0.064
#&gt; GSM38715     1  0.4332      0.816 0.788 0.000 0.016 0.132 0.064
#&gt; GSM38716     1  0.0324      0.880 0.992 0.000 0.000 0.004 0.004
#&gt; GSM38717     1  0.2889      0.850 0.872 0.000 0.000 0.084 0.044
#&gt; GSM38718     1  0.3593      0.832 0.824 0.000 0.000 0.116 0.060
#&gt; GSM38719     1  0.1701      0.871 0.936 0.000 0.000 0.048 0.016
#&gt; GSM38720     1  0.0798      0.879 0.976 0.000 0.000 0.016 0.008
#&gt; GSM38721     1  0.4425      0.814 0.784 0.000 0.020 0.132 0.064
#&gt; GSM38722     1  0.0671      0.875 0.980 0.000 0.000 0.016 0.004
#&gt; GSM38723     1  0.0671      0.875 0.980 0.000 0.000 0.016 0.004
#&gt; GSM38724     3  0.1200      0.945 0.012 0.000 0.964 0.016 0.008
#&gt; GSM38725     1  0.0693      0.879 0.980 0.000 0.000 0.008 0.012
#&gt; GSM38726     1  0.0290      0.878 0.992 0.000 0.000 0.008 0.000
#&gt; GSM38727     1  0.0671      0.875 0.980 0.000 0.000 0.016 0.004
#&gt; GSM38728     3  0.1770      0.933 0.008 0.000 0.936 0.048 0.008
#&gt; GSM38729     1  0.0162      0.880 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38730     1  0.0290      0.878 0.992 0.000 0.000 0.008 0.000
#&gt; GSM38731     1  0.0290      0.878 0.992 0.000 0.000 0.008 0.000
#&gt; GSM38732     3  0.1965      0.933 0.000 0.000 0.924 0.052 0.024
#&gt; GSM38733     1  0.4514      0.813 0.780 0.000 0.024 0.132 0.064
#&gt; GSM38734     4  0.3354      0.862 0.000 0.024 0.140 0.832 0.004
#&gt; GSM38735     5  0.3443      0.955 0.044 0.092 0.008 0.004 0.852
#&gt; GSM38736     2  0.0290      0.965 0.000 0.992 0.000 0.000 0.008
#&gt; GSM38737     2  0.0290      0.965 0.000 0.992 0.000 0.000 0.008
#&gt; GSM38738     3  0.1893      0.934 0.000 0.000 0.928 0.048 0.024
#&gt; GSM38739     1  0.4690      0.653 0.724 0.000 0.224 0.016 0.036
#&gt; GSM38740     5  0.3202      0.955 0.080 0.056 0.004 0.000 0.860
#&gt; GSM38741     3  0.1626      0.925 0.000 0.000 0.940 0.044 0.016
#&gt; GSM38742     2  0.1251      0.953 0.000 0.956 0.008 0.036 0.000
#&gt; GSM38743     2  0.0290      0.965 0.000 0.992 0.000 0.000 0.008
#&gt; GSM38744     5  0.3191      0.951 0.084 0.052 0.004 0.000 0.860
#&gt; GSM38745     5  0.3443      0.955 0.044 0.092 0.008 0.004 0.852
#&gt; GSM38746     3  0.1200      0.944 0.012 0.000 0.964 0.008 0.016
#&gt; GSM38747     3  0.1095      0.945 0.012 0.000 0.968 0.008 0.012
#&gt; GSM38748     4  0.4293      0.865 0.000 0.092 0.076 0.804 0.028
#&gt; GSM38749     1  0.4660      0.659 0.728 0.000 0.220 0.016 0.036
#&gt; GSM38750     3  0.1596      0.943 0.012 0.000 0.948 0.012 0.028
#&gt; GSM38751     3  0.1195      0.946 0.012 0.000 0.960 0.000 0.028
#&gt; GSM38752     4  0.3631      0.864 0.000 0.024 0.144 0.820 0.012
#&gt; GSM38753     4  0.3687      0.810 0.000 0.180 0.000 0.792 0.028
#&gt; GSM38754     4  0.3631      0.864 0.000 0.024 0.144 0.820 0.012
#&gt; GSM38755     3  0.3647      0.872 0.020 0.000 0.844 0.068 0.068
#&gt; GSM38756     4  0.3687      0.810 0.000 0.180 0.000 0.792 0.028
#&gt; GSM38757     3  0.1314      0.945 0.012 0.000 0.960 0.012 0.016
#&gt; GSM38758     2  0.1628      0.933 0.000 0.936 0.008 0.056 0.000
#&gt; GSM38759     3  0.2900      0.879 0.020 0.000 0.876 0.092 0.012
#&gt; GSM38760     1  0.4296      0.680 0.756 0.000 0.204 0.016 0.024
#&gt; GSM38761     2  0.1251      0.953 0.000 0.956 0.008 0.036 0.000
#&gt; GSM38762     2  0.0290      0.965 0.000 0.992 0.000 0.000 0.008
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1556      0.599 0.920 0.000 0.000 0.000 0.000 0.080
#&gt; GSM38713     6  0.3950      0.943 0.432 0.000 0.004 0.000 0.000 0.564
#&gt; GSM38714     6  0.4403      0.922 0.408 0.000 0.028 0.000 0.000 0.564
#&gt; GSM38715     6  0.3950      0.943 0.432 0.000 0.004 0.000 0.000 0.564
#&gt; GSM38716     1  0.1556      0.599 0.920 0.000 0.000 0.000 0.000 0.080
#&gt; GSM38717     1  0.3774     -0.550 0.592 0.000 0.000 0.000 0.000 0.408
#&gt; GSM38718     1  0.3864     -0.763 0.520 0.000 0.000 0.000 0.000 0.480
#&gt; GSM38719     1  0.3050      0.220 0.764 0.000 0.000 0.000 0.000 0.236
#&gt; GSM38720     1  0.2562      0.424 0.828 0.000 0.000 0.000 0.000 0.172
#&gt; GSM38721     6  0.3961      0.931 0.440 0.000 0.004 0.000 0.000 0.556
#&gt; GSM38722     1  0.1204      0.649 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM38723     1  0.1204      0.649 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM38724     3  0.2135      0.843 0.000 0.000 0.872 0.000 0.000 0.128
#&gt; GSM38725     1  0.0146      0.659 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM38726     1  0.0000      0.660 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.1204      0.649 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM38728     3  0.3342      0.790 0.000 0.000 0.760 0.012 0.000 0.228
#&gt; GSM38729     1  0.1267      0.620 0.940 0.000 0.000 0.000 0.000 0.060
#&gt; GSM38730     1  0.0000      0.660 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.660 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.2620      0.840 0.000 0.000 0.868 0.012 0.012 0.108
#&gt; GSM38733     6  0.4747      0.867 0.376 0.000 0.056 0.000 0.000 0.568
#&gt; GSM38734     4  0.2665      0.915 0.000 0.000 0.012 0.868 0.016 0.104
#&gt; GSM38735     5  0.1109      0.990 0.004 0.012 0.000 0.004 0.964 0.016
#&gt; GSM38736     2  0.0547      0.973 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38737     2  0.0547      0.973 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38738     3  0.2114      0.847 0.000 0.000 0.904 0.008 0.012 0.076
#&gt; GSM38739     1  0.5316      0.362 0.592 0.000 0.240 0.000 0.000 0.168
#&gt; GSM38740     5  0.0508      0.989 0.004 0.012 0.000 0.000 0.984 0.000
#&gt; GSM38741     3  0.2664      0.832 0.000 0.000 0.848 0.016 0.000 0.136
#&gt; GSM38742     2  0.1194      0.964 0.000 0.956 0.000 0.008 0.004 0.032
#&gt; GSM38743     2  0.0547      0.973 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38744     5  0.0520      0.987 0.008 0.008 0.000 0.000 0.984 0.000
#&gt; GSM38745     5  0.1109      0.990 0.004 0.012 0.000 0.004 0.964 0.016
#&gt; GSM38746     3  0.2738      0.833 0.000 0.000 0.820 0.004 0.000 0.176
#&gt; GSM38747     3  0.2848      0.835 0.000 0.000 0.816 0.008 0.000 0.176
#&gt; GSM38748     4  0.0964      0.919 0.000 0.004 0.012 0.968 0.000 0.016
#&gt; GSM38749     1  0.5275      0.370 0.600 0.000 0.232 0.000 0.000 0.168
#&gt; GSM38750     3  0.2685      0.822 0.000 0.000 0.852 0.008 0.008 0.132
#&gt; GSM38751     3  0.1910      0.827 0.000 0.000 0.892 0.000 0.000 0.108
#&gt; GSM38752     4  0.2500      0.915 0.000 0.000 0.012 0.868 0.004 0.116
#&gt; GSM38753     4  0.0937      0.917 0.000 0.040 0.000 0.960 0.000 0.000
#&gt; GSM38754     4  0.2500      0.915 0.000 0.000 0.012 0.868 0.004 0.116
#&gt; GSM38755     3  0.2742      0.826 0.000 0.000 0.852 0.008 0.012 0.128
#&gt; GSM38756     4  0.0937      0.917 0.000 0.040 0.000 0.960 0.000 0.000
#&gt; GSM38757     3  0.1149      0.853 0.000 0.000 0.960 0.008 0.008 0.024
#&gt; GSM38758     2  0.1151      0.960 0.000 0.956 0.000 0.012 0.000 0.032
#&gt; GSM38759     3  0.3804      0.679 0.000 0.000 0.656 0.008 0.000 0.336
#&gt; GSM38760     1  0.5095      0.396 0.632 0.000 0.188 0.000 0.000 0.180
#&gt; GSM38761     2  0.1049      0.963 0.000 0.960 0.000 0.008 0.000 0.032
#&gt; GSM38762     2  0.0547      0.973 0.000 0.980 0.000 0.000 0.020 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n individual(p) k
#> SD:kmeans 51      0.046137 2
#> SD:kmeans 38      0.007908 3
#> SD:kmeans 46      0.001667 4
#> SD:kmeans 51      0.000942 5
#> SD:kmeans 44      0.001207 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.957           0.965       0.982         0.4972 0.506   0.506
#> 3 3 0.918           0.932       0.972         0.3126 0.699   0.477
#> 4 4 0.828           0.823       0.907         0.1173 0.897   0.711
#> 5 5 0.749           0.747       0.821         0.0618 0.978   0.917
#> 6 6 0.777           0.751       0.853         0.0555 0.878   0.552
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.971 1.000 0.000
#&gt; GSM38713     1   0.000      0.971 1.000 0.000
#&gt; GSM38714     1   0.000      0.971 1.000 0.000
#&gt; GSM38715     1   0.000      0.971 1.000 0.000
#&gt; GSM38716     1   0.000      0.971 1.000 0.000
#&gt; GSM38717     1   0.000      0.971 1.000 0.000
#&gt; GSM38718     1   0.000      0.971 1.000 0.000
#&gt; GSM38719     1   0.000      0.971 1.000 0.000
#&gt; GSM38720     1   0.000      0.971 1.000 0.000
#&gt; GSM38721     1   0.000      0.971 1.000 0.000
#&gt; GSM38722     1   0.000      0.971 1.000 0.000
#&gt; GSM38723     1   0.000      0.971 1.000 0.000
#&gt; GSM38724     1   0.518      0.873 0.884 0.116
#&gt; GSM38725     1   0.000      0.971 1.000 0.000
#&gt; GSM38726     1   0.000      0.971 1.000 0.000
#&gt; GSM38727     1   0.000      0.971 1.000 0.000
#&gt; GSM38728     2   0.000      0.995 0.000 1.000
#&gt; GSM38729     1   0.000      0.971 1.000 0.000
#&gt; GSM38730     1   0.000      0.971 1.000 0.000
#&gt; GSM38731     1   0.000      0.971 1.000 0.000
#&gt; GSM38732     2   0.000      0.995 0.000 1.000
#&gt; GSM38733     1   0.000      0.971 1.000 0.000
#&gt; GSM38734     2   0.000      0.995 0.000 1.000
#&gt; GSM38735     2   0.430      0.899 0.088 0.912
#&gt; GSM38736     2   0.000      0.995 0.000 1.000
#&gt; GSM38737     2   0.000      0.995 0.000 1.000
#&gt; GSM38738     2   0.000      0.995 0.000 1.000
#&gt; GSM38739     1   0.000      0.971 1.000 0.000
#&gt; GSM38740     1   0.000      0.971 1.000 0.000
#&gt; GSM38741     2   0.000      0.995 0.000 1.000
#&gt; GSM38742     2   0.000      0.995 0.000 1.000
#&gt; GSM38743     2   0.000      0.995 0.000 1.000
#&gt; GSM38744     1   0.000      0.971 1.000 0.000
#&gt; GSM38745     1   0.402      0.908 0.920 0.080
#&gt; GSM38746     1   0.722      0.779 0.800 0.200
#&gt; GSM38747     1   0.722      0.779 0.800 0.200
#&gt; GSM38748     2   0.000      0.995 0.000 1.000
#&gt; GSM38749     1   0.000      0.971 1.000 0.000
#&gt; GSM38750     2   0.000      0.995 0.000 1.000
#&gt; GSM38751     2   0.000      0.995 0.000 1.000
#&gt; GSM38752     2   0.000      0.995 0.000 1.000
#&gt; GSM38753     2   0.000      0.995 0.000 1.000
#&gt; GSM38754     2   0.000      0.995 0.000 1.000
#&gt; GSM38755     1   0.141      0.957 0.980 0.020
#&gt; GSM38756     2   0.000      0.995 0.000 1.000
#&gt; GSM38757     2   0.000      0.995 0.000 1.000
#&gt; GSM38758     2   0.000      0.995 0.000 1.000
#&gt; GSM38759     1   0.722      0.779 0.800 0.200
#&gt; GSM38760     1   0.000      0.971 1.000 0.000
#&gt; GSM38761     2   0.000      0.995 0.000 1.000
#&gt; GSM38762     2   0.000      0.995 0.000 1.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38713     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38714     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38715     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38716     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38717     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38718     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38719     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38720     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38721     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38722     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38723     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38724     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38725     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38726     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38727     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38728     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38729     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38730     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38731     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38732     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38733     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38734     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38735     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38736     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38737     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38738     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38739     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38740     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38741     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38742     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38743     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38744     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38745     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38746     3   0.327     0.8203 0.116 0.000 0.884
#&gt; GSM38747     3   0.327     0.8203 0.116 0.000 0.884
#&gt; GSM38748     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38749     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38750     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38751     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38752     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38753     3   0.455     0.7231 0.000 0.200 0.800
#&gt; GSM38754     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38755     3   0.568     0.5605 0.316 0.000 0.684
#&gt; GSM38756     3   0.450     0.7281 0.000 0.196 0.804
#&gt; GSM38757     3   0.000     0.9012 0.000 0.000 1.000
#&gt; GSM38758     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38759     3   0.631     0.0798 0.492 0.000 0.508
#&gt; GSM38760     1   0.000     1.0000 1.000 0.000 0.000
#&gt; GSM38761     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM38762     2   0.000     1.0000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.1867      0.903 0.928 0.000 0.072 0.000
#&gt; GSM38714     1  0.1867      0.903 0.928 0.000 0.072 0.000
#&gt; GSM38715     1  0.1867      0.903 0.928 0.000 0.072 0.000
#&gt; GSM38716     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.1867      0.903 0.928 0.000 0.072 0.000
#&gt; GSM38722     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.0592      0.749 0.000 0.000 0.984 0.016
#&gt; GSM38725     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38728     4  0.4564      0.677 0.000 0.000 0.328 0.672
#&gt; GSM38729     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38732     4  0.2868      0.814 0.000 0.000 0.136 0.864
#&gt; GSM38733     1  0.1867      0.903 0.928 0.000 0.072 0.000
#&gt; GSM38734     4  0.0707      0.856 0.000 0.000 0.020 0.980
#&gt; GSM38735     2  0.0000      0.902 0.000 1.000 0.000 0.000
#&gt; GSM38736     2  0.2868      0.937 0.000 0.864 0.000 0.136
#&gt; GSM38737     2  0.2868      0.937 0.000 0.864 0.000 0.136
#&gt; GSM38738     4  0.2868      0.814 0.000 0.000 0.136 0.864
#&gt; GSM38739     3  0.4941      0.354 0.436 0.000 0.564 0.000
#&gt; GSM38740     2  0.0000      0.902 0.000 1.000 0.000 0.000
#&gt; GSM38741     4  0.4103      0.726 0.000 0.000 0.256 0.744
#&gt; GSM38742     2  0.2868      0.937 0.000 0.864 0.000 0.136
#&gt; GSM38743     2  0.2868      0.937 0.000 0.864 0.000 0.136
#&gt; GSM38744     2  0.0000      0.902 0.000 1.000 0.000 0.000
#&gt; GSM38745     2  0.0000      0.902 0.000 1.000 0.000 0.000
#&gt; GSM38746     3  0.0000      0.754 0.000 0.000 1.000 0.000
#&gt; GSM38747     3  0.0000      0.754 0.000 0.000 1.000 0.000
#&gt; GSM38748     4  0.0336      0.851 0.000 0.000 0.008 0.992
#&gt; GSM38749     3  0.4941      0.354 0.436 0.000 0.564 0.000
#&gt; GSM38750     3  0.1867      0.745 0.000 0.000 0.928 0.072
#&gt; GSM38751     3  0.1867      0.745 0.000 0.000 0.928 0.072
#&gt; GSM38752     4  0.0707      0.856 0.000 0.000 0.020 0.980
#&gt; GSM38753     4  0.0000      0.847 0.000 0.000 0.000 1.000
#&gt; GSM38754     4  0.0707      0.856 0.000 0.000 0.020 0.980
#&gt; GSM38755     3  0.6027      0.638 0.192 0.000 0.684 0.124
#&gt; GSM38756     4  0.0000      0.847 0.000 0.000 0.000 1.000
#&gt; GSM38757     3  0.2011      0.740 0.000 0.000 0.920 0.080
#&gt; GSM38758     2  0.3688      0.875 0.000 0.792 0.000 0.208
#&gt; GSM38759     4  0.7517      0.395 0.212 0.000 0.304 0.484
#&gt; GSM38760     1  0.5000     -0.274 0.500 0.000 0.500 0.000
#&gt; GSM38761     2  0.2868      0.937 0.000 0.864 0.000 0.136
#&gt; GSM38762     2  0.2868      0.937 0.000 0.864 0.000 0.136
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0404      0.804 0.988 0.000 0.000 0.000 0.012
#&gt; GSM38713     1  0.5873      0.602 0.556 0.000 0.024 0.056 0.364
#&gt; GSM38714     1  0.5873      0.602 0.556 0.000 0.024 0.056 0.364
#&gt; GSM38715     1  0.5873      0.602 0.556 0.000 0.024 0.056 0.364
#&gt; GSM38716     1  0.0510      0.804 0.984 0.000 0.000 0.000 0.016
#&gt; GSM38717     1  0.3011      0.761 0.844 0.000 0.000 0.016 0.140
#&gt; GSM38718     1  0.4645      0.682 0.688 0.000 0.000 0.044 0.268
#&gt; GSM38719     1  0.2124      0.782 0.900 0.000 0.000 0.004 0.096
#&gt; GSM38720     1  0.0794      0.802 0.972 0.000 0.000 0.000 0.028
#&gt; GSM38721     1  0.5873      0.602 0.556 0.000 0.024 0.056 0.364
#&gt; GSM38722     1  0.0290      0.799 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38723     1  0.0451      0.797 0.988 0.000 0.004 0.000 0.008
#&gt; GSM38724     3  0.3885      0.624 0.000 0.000 0.784 0.040 0.176
#&gt; GSM38725     1  0.0000      0.803 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.803 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0290      0.799 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38728     4  0.5798      0.546 0.000 0.000 0.156 0.608 0.236
#&gt; GSM38729     1  0.0162      0.803 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38730     1  0.0000      0.803 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.803 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     4  0.1408      0.803 0.000 0.000 0.044 0.948 0.008
#&gt; GSM38733     1  0.5792      0.610 0.568 0.000 0.024 0.052 0.356
#&gt; GSM38734     4  0.1341      0.824 0.000 0.056 0.000 0.944 0.000
#&gt; GSM38735     5  0.4268      1.000 0.000 0.444 0.000 0.000 0.556
#&gt; GSM38736     2  0.0000      0.975 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.975 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38738     4  0.1626      0.801 0.000 0.000 0.044 0.940 0.016
#&gt; GSM38739     3  0.4656      0.259 0.480 0.000 0.508 0.000 0.012
#&gt; GSM38740     5  0.4268      1.000 0.000 0.444 0.000 0.000 0.556
#&gt; GSM38741     4  0.3710      0.727 0.000 0.000 0.144 0.808 0.048
#&gt; GSM38742     2  0.0290      0.971 0.000 0.992 0.000 0.008 0.000
#&gt; GSM38743     2  0.0000      0.975 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.4268      1.000 0.000 0.444 0.000 0.000 0.556
#&gt; GSM38745     5  0.4268      1.000 0.000 0.444 0.000 0.000 0.556
#&gt; GSM38746     3  0.1121      0.735 0.000 0.000 0.956 0.000 0.044
#&gt; GSM38747     3  0.1410      0.730 0.000 0.000 0.940 0.000 0.060
#&gt; GSM38748     4  0.2561      0.794 0.000 0.144 0.000 0.856 0.000
#&gt; GSM38749     3  0.4656      0.259 0.480 0.000 0.508 0.000 0.012
#&gt; GSM38750     3  0.1626      0.740 0.000 0.000 0.940 0.044 0.016
#&gt; GSM38751     3  0.0880      0.743 0.000 0.000 0.968 0.032 0.000
#&gt; GSM38752     4  0.1544      0.824 0.000 0.068 0.000 0.932 0.000
#&gt; GSM38753     4  0.2852      0.774 0.000 0.172 0.000 0.828 0.000
#&gt; GSM38754     4  0.1544      0.824 0.000 0.068 0.000 0.932 0.000
#&gt; GSM38755     3  0.5288      0.658 0.176 0.000 0.712 0.088 0.024
#&gt; GSM38756     4  0.2813      0.778 0.000 0.168 0.000 0.832 0.000
#&gt; GSM38757     3  0.2110      0.730 0.000 0.000 0.912 0.072 0.016
#&gt; GSM38758     2  0.1270      0.890 0.000 0.948 0.000 0.052 0.000
#&gt; GSM38759     4  0.7664      0.208 0.128 0.000 0.104 0.388 0.380
#&gt; GSM38760     1  0.4582     -0.102 0.572 0.000 0.416 0.000 0.012
#&gt; GSM38761     2  0.0290      0.971 0.000 0.992 0.000 0.008 0.000
#&gt; GSM38762     2  0.0000      0.975 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1714     0.7780 0.908 0.000 0.000 0.000 0.000 0.092
#&gt; GSM38713     6  0.2730     0.7805 0.192 0.000 0.000 0.000 0.000 0.808
#&gt; GSM38714     6  0.2730     0.7805 0.192 0.000 0.000 0.000 0.000 0.808
#&gt; GSM38715     6  0.2730     0.7805 0.192 0.000 0.000 0.000 0.000 0.808
#&gt; GSM38716     1  0.1556     0.7867 0.920 0.000 0.000 0.000 0.000 0.080
#&gt; GSM38717     1  0.3860    -0.0914 0.528 0.000 0.000 0.000 0.000 0.472
#&gt; GSM38718     6  0.3499     0.5918 0.320 0.000 0.000 0.000 0.000 0.680
#&gt; GSM38719     1  0.3482     0.4268 0.684 0.000 0.000 0.000 0.000 0.316
#&gt; GSM38720     1  0.2300     0.7266 0.856 0.000 0.000 0.000 0.000 0.144
#&gt; GSM38721     6  0.2730     0.7805 0.192 0.000 0.000 0.000 0.000 0.808
#&gt; GSM38722     1  0.0260     0.8069 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM38723     1  0.0260     0.8069 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM38724     3  0.4724     0.5962 0.000 0.000 0.616 0.012 0.040 0.332
#&gt; GSM38725     1  0.0547     0.8127 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM38726     1  0.0547     0.8127 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM38727     1  0.0260     0.8069 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM38728     6  0.6367    -0.1989 0.000 0.000 0.168 0.372 0.032 0.428
#&gt; GSM38729     1  0.1204     0.8001 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM38730     1  0.0547     0.8127 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM38731     1  0.0547     0.8127 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM38732     4  0.3114     0.7767 0.000 0.000 0.048 0.860 0.040 0.052
#&gt; GSM38733     6  0.2631     0.7698 0.180 0.000 0.000 0.000 0.000 0.820
#&gt; GSM38734     4  0.0291     0.8372 0.000 0.000 0.004 0.992 0.004 0.000
#&gt; GSM38735     5  0.1663     0.9979 0.000 0.088 0.000 0.000 0.912 0.000
#&gt; GSM38736     2  0.0000     0.9851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000     0.9851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38738     4  0.3368     0.7637 0.000 0.000 0.056 0.844 0.044 0.056
#&gt; GSM38739     1  0.4124     0.4570 0.648 0.000 0.332 0.000 0.012 0.008
#&gt; GSM38740     5  0.1663     0.9979 0.000 0.088 0.000 0.000 0.912 0.000
#&gt; GSM38741     4  0.4700     0.6219 0.000 0.000 0.168 0.716 0.020 0.096
#&gt; GSM38742     2  0.0363     0.9820 0.000 0.988 0.000 0.012 0.000 0.000
#&gt; GSM38743     2  0.0000     0.9851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.1753     0.9936 0.004 0.084 0.000 0.000 0.912 0.000
#&gt; GSM38745     5  0.1663     0.9979 0.000 0.088 0.000 0.000 0.912 0.000
#&gt; GSM38746     3  0.2527     0.7668 0.000 0.000 0.880 0.004 0.032 0.084
#&gt; GSM38747     3  0.3314     0.7442 0.000 0.000 0.816 0.008 0.032 0.144
#&gt; GSM38748     4  0.2146     0.8305 0.000 0.116 0.000 0.880 0.004 0.000
#&gt; GSM38749     1  0.4124     0.4570 0.648 0.000 0.332 0.000 0.012 0.008
#&gt; GSM38750     3  0.2875     0.7712 0.000 0.000 0.872 0.064 0.036 0.028
#&gt; GSM38751     3  0.0603     0.7829 0.000 0.000 0.980 0.016 0.000 0.004
#&gt; GSM38752     4  0.1542     0.8446 0.000 0.024 0.016 0.944 0.000 0.016
#&gt; GSM38753     4  0.2697     0.7864 0.000 0.188 0.000 0.812 0.000 0.000
#&gt; GSM38754     4  0.1622     0.8448 0.000 0.028 0.016 0.940 0.000 0.016
#&gt; GSM38755     3  0.6345     0.6222 0.140 0.000 0.632 0.116 0.048 0.064
#&gt; GSM38756     4  0.2664     0.7903 0.000 0.184 0.000 0.816 0.000 0.000
#&gt; GSM38757     3  0.3763     0.7633 0.000 0.000 0.816 0.080 0.044 0.060
#&gt; GSM38758     2  0.1007     0.9479 0.000 0.956 0.000 0.044 0.000 0.000
#&gt; GSM38759     6  0.5366     0.4005 0.032 0.000 0.112 0.124 0.028 0.704
#&gt; GSM38760     1  0.3704     0.5960 0.744 0.000 0.232 0.000 0.016 0.008
#&gt; GSM38761     2  0.0363     0.9820 0.000 0.988 0.000 0.012 0.000 0.000
#&gt; GSM38762     2  0.0000     0.9851 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n individual(p) k
#> SD:skmeans 51      0.078777 2
#> SD:skmeans 50      0.000260 3
#> SD:skmeans 47      0.000834 4
#> SD:skmeans 47      0.000199 5
#> SD:skmeans 45      0.000932 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.958           0.926       0.972         0.3806 0.613   0.613
#> 3 3 0.848           0.867       0.950         0.6830 0.663   0.484
#> 4 4 0.972           0.954       0.980         0.1122 0.889   0.708
#> 5 5 1.000           0.967       0.986         0.0507 0.955   0.846
#> 6 6 0.995           0.952       0.975         0.0980 0.870   0.535
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 4 5
```

There is also optional best $k$ = 2 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.983 1.000 0.000
#&gt; GSM38713     1  0.0000      0.983 1.000 0.000
#&gt; GSM38714     1  0.0000      0.983 1.000 0.000
#&gt; GSM38715     1  0.0000      0.983 1.000 0.000
#&gt; GSM38716     1  0.0000      0.983 1.000 0.000
#&gt; GSM38717     1  0.0000      0.983 1.000 0.000
#&gt; GSM38718     1  0.0000      0.983 1.000 0.000
#&gt; GSM38719     1  0.0000      0.983 1.000 0.000
#&gt; GSM38720     1  0.0000      0.983 1.000 0.000
#&gt; GSM38721     1  0.0000      0.983 1.000 0.000
#&gt; GSM38722     1  0.0000      0.983 1.000 0.000
#&gt; GSM38723     1  0.0000      0.983 1.000 0.000
#&gt; GSM38724     1  0.0000      0.983 1.000 0.000
#&gt; GSM38725     1  0.0000      0.983 1.000 0.000
#&gt; GSM38726     1  0.0000      0.983 1.000 0.000
#&gt; GSM38727     1  0.0000      0.983 1.000 0.000
#&gt; GSM38728     1  0.0000      0.983 1.000 0.000
#&gt; GSM38729     1  0.0000      0.983 1.000 0.000
#&gt; GSM38730     1  0.0000      0.983 1.000 0.000
#&gt; GSM38731     1  0.0000      0.983 1.000 0.000
#&gt; GSM38732     1  0.0000      0.983 1.000 0.000
#&gt; GSM38733     1  0.0000      0.983 1.000 0.000
#&gt; GSM38734     1  0.0000      0.983 1.000 0.000
#&gt; GSM38735     2  0.1184      0.916 0.016 0.984
#&gt; GSM38736     2  0.0000      0.925 0.000 1.000
#&gt; GSM38737     2  0.0000      0.925 0.000 1.000
#&gt; GSM38738     1  0.0000      0.983 1.000 0.000
#&gt; GSM38739     1  0.0000      0.983 1.000 0.000
#&gt; GSM38740     2  0.9881      0.262 0.436 0.564
#&gt; GSM38741     1  0.0000      0.983 1.000 0.000
#&gt; GSM38742     2  0.0000      0.925 0.000 1.000
#&gt; GSM38743     2  0.0000      0.925 0.000 1.000
#&gt; GSM38744     1  0.0376      0.979 0.996 0.004
#&gt; GSM38745     2  0.1184      0.916 0.016 0.984
#&gt; GSM38746     1  0.0000      0.983 1.000 0.000
#&gt; GSM38747     1  0.0000      0.983 1.000 0.000
#&gt; GSM38748     2  0.9661      0.379 0.392 0.608
#&gt; GSM38749     1  0.0000      0.983 1.000 0.000
#&gt; GSM38750     1  0.0000      0.983 1.000 0.000
#&gt; GSM38751     1  0.0000      0.983 1.000 0.000
#&gt; GSM38752     1  0.7139      0.727 0.804 0.196
#&gt; GSM38753     2  0.0000      0.925 0.000 1.000
#&gt; GSM38754     1  0.9608      0.311 0.616 0.384
#&gt; GSM38755     1  0.0000      0.983 1.000 0.000
#&gt; GSM38756     2  0.0000      0.925 0.000 1.000
#&gt; GSM38757     1  0.0000      0.983 1.000 0.000
#&gt; GSM38758     2  0.0000      0.925 0.000 1.000
#&gt; GSM38759     1  0.0000      0.983 1.000 0.000
#&gt; GSM38760     1  0.0000      0.983 1.000 0.000
#&gt; GSM38761     2  0.0000      0.925 0.000 1.000
#&gt; GSM38762     2  0.0000      0.925 0.000 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38714     1  0.5835      0.457 0.660 0.000 0.340
#&gt; GSM38715     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38721     1  0.3752      0.809 0.856 0.000 0.144
#&gt; GSM38722     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38724     3  0.0424      0.896 0.008 0.000 0.992
#&gt; GSM38725     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38728     3  0.0000      0.894 0.000 0.000 1.000
#&gt; GSM38729     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38732     3  0.0424      0.896 0.008 0.000 0.992
#&gt; GSM38733     1  0.1163      0.940 0.972 0.000 0.028
#&gt; GSM38734     3  0.0000      0.894 0.000 0.000 1.000
#&gt; GSM38735     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM38736     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM38738     3  0.0424      0.896 0.008 0.000 0.992
#&gt; GSM38739     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM38740     1  0.5291      0.629 0.732 0.268 0.000
#&gt; GSM38741     3  0.0000      0.894 0.000 0.000 1.000
#&gt; GSM38742     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM38744     1  0.0000      0.963 1.000 0.000 0.000
#&gt; GSM38745     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM38746     3  0.5905      0.445 0.352 0.000 0.648
#&gt; GSM38747     3  0.5905      0.445 0.352 0.000 0.648
#&gt; GSM38748     3  0.0237      0.892 0.000 0.004 0.996
#&gt; GSM38749     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM38750     3  0.0424      0.896 0.008 0.000 0.992
#&gt; GSM38751     3  0.0424      0.896 0.008 0.000 0.992
#&gt; GSM38752     3  0.0000      0.894 0.000 0.000 1.000
#&gt; GSM38753     3  0.6302     -0.078 0.000 0.480 0.520
#&gt; GSM38754     3  0.0000      0.894 0.000 0.000 1.000
#&gt; GSM38755     3  0.2165      0.849 0.064 0.000 0.936
#&gt; GSM38756     2  0.6180      0.292 0.000 0.584 0.416
#&gt; GSM38757     3  0.0424      0.896 0.008 0.000 0.992
#&gt; GSM38758     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM38759     3  0.1289      0.879 0.032 0.000 0.968
#&gt; GSM38760     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM38761     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000      0.951 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38713     1   0.139      0.931 0.952 0.000 0.048 0.000
#&gt; GSM38714     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38715     1   0.139      0.931 0.952 0.000 0.048 0.000
#&gt; GSM38716     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38717     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38718     1   0.139      0.931 0.952 0.000 0.048 0.000
#&gt; GSM38719     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38720     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38721     3   0.265      0.825 0.120 0.000 0.880 0.000
#&gt; GSM38722     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38723     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38724     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38725     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38726     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38727     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38728     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38729     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38730     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38731     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38732     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38733     1   0.404      0.679 0.752 0.000 0.248 0.000
#&gt; GSM38734     4   0.000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM38735     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38736     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38737     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38738     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38739     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38740     1   0.455      0.649 0.732 0.256 0.012 0.000
#&gt; GSM38741     3   0.401      0.684 0.000 0.000 0.756 0.244
#&gt; GSM38742     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38743     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38744     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38745     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38746     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38747     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38748     4   0.000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM38749     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38750     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38751     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38752     4   0.000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM38753     4   0.000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM38754     4   0.000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM38755     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38756     4   0.000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM38757     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38758     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38759     3   0.000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38760     1   0.000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM38761     2   0.000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38762     2   0.000      1.000 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4 p5
#&gt; GSM38712     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38713     1  0.1197      0.941 0.952  0 0.048 0.000  0
#&gt; GSM38714     3  0.0162      0.963 0.004  0 0.996 0.000  0
#&gt; GSM38715     1  0.1197      0.941 0.952  0 0.048 0.000  0
#&gt; GSM38716     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38717     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38718     1  0.1197      0.941 0.952  0 0.048 0.000  0
#&gt; GSM38719     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38720     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38721     3  0.2329      0.812 0.124  0 0.876 0.000  0
#&gt; GSM38722     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38723     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38724     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38725     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38726     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38727     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38728     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38729     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38730     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38731     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38732     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38733     1  0.3143      0.738 0.796  0 0.204 0.000  0
#&gt; GSM38734     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38735     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38736     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38737     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38738     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38739     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38740     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38741     3  0.3452      0.684 0.000  0 0.756 0.244  0
#&gt; GSM38742     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38743     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38744     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38745     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38746     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38747     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38748     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38749     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38750     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38751     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38752     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38753     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38754     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38755     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38756     4  0.0000      1.000 0.000  0 0.000 1.000  0
#&gt; GSM38757     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38758     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38759     3  0.0000      0.967 0.000  0 1.000 0.000  0
#&gt; GSM38760     1  0.0000      0.979 1.000  0 0.000 0.000  0
#&gt; GSM38761     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38762     2  0.0000      1.000 0.000  1 0.000 0.000  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4 p5    p6
#&gt; GSM38712     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38713     6  0.0865      0.960 0.036  0 0.000 0.000  0 0.964
#&gt; GSM38714     6  0.0865      0.972 0.000  0 0.036 0.000  0 0.964
#&gt; GSM38715     6  0.0865      0.960 0.036  0 0.000 0.000  0 0.964
#&gt; GSM38716     1  0.0146      0.965 0.996  0 0.000 0.000  0 0.004
#&gt; GSM38717     1  0.3717      0.354 0.616  0 0.000 0.000  0 0.384
#&gt; GSM38718     6  0.0865      0.960 0.036  0 0.000 0.000  0 0.964
#&gt; GSM38719     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38720     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38721     6  0.0865      0.972 0.000  0 0.036 0.000  0 0.964
#&gt; GSM38722     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38723     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38724     6  0.0937      0.971 0.000  0 0.040 0.000  0 0.960
#&gt; GSM38725     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38726     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38727     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38728     6  0.0937      0.971 0.000  0 0.040 0.000  0 0.960
#&gt; GSM38729     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38730     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38731     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38732     3  0.1765      0.867 0.000  0 0.904 0.000  0 0.096
#&gt; GSM38733     6  0.1168      0.966 0.028  0 0.016 0.000  0 0.956
#&gt; GSM38734     4  0.0865      0.978 0.000  0 0.000 0.964  0 0.036
#&gt; GSM38735     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38738     3  0.0000      0.940 0.000  0 1.000 0.000  0 0.000
#&gt; GSM38739     3  0.2416      0.764 0.156  0 0.844 0.000  0 0.000
#&gt; GSM38740     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM38741     4  0.1007      0.946 0.000  0 0.044 0.956  0 0.000
#&gt; GSM38742     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38744     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM38745     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM38746     3  0.0000      0.940 0.000  0 1.000 0.000  0 0.000
#&gt; GSM38747     3  0.2048      0.839 0.000  0 0.880 0.000  0 0.120
#&gt; GSM38748     4  0.0000      0.978 0.000  0 0.000 1.000  0 0.000
#&gt; GSM38749     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38750     3  0.0000      0.940 0.000  0 1.000 0.000  0 0.000
#&gt; GSM38751     3  0.0000      0.940 0.000  0 1.000 0.000  0 0.000
#&gt; GSM38752     4  0.0865      0.978 0.000  0 0.000 0.964  0 0.036
#&gt; GSM38753     4  0.0000      0.978 0.000  0 0.000 1.000  0 0.000
#&gt; GSM38754     4  0.0865      0.978 0.000  0 0.000 0.964  0 0.036
#&gt; GSM38755     3  0.0000      0.940 0.000  0 1.000 0.000  0 0.000
#&gt; GSM38756     4  0.0000      0.978 0.000  0 0.000 1.000  0 0.000
#&gt; GSM38757     3  0.0000      0.940 0.000  0 1.000 0.000  0 0.000
#&gt; GSM38758     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38759     6  0.0937      0.971 0.000  0 0.040 0.000  0 0.960
#&gt; GSM38760     1  0.0000      0.968 1.000  0 0.000 0.000  0 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n individual(p) k
#> SD:pam 48       0.02976 2
#> SD:pam 46       0.00805 3
#> SD:pam 51       0.01392 4
#> SD:pam 51       0.00372 5
#> SD:pam 50       0.00107 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.824           0.882       0.938         0.4445 0.534   0.534
#> 3 3 0.596           0.551       0.812         0.3250 0.721   0.522
#> 4 4 0.586           0.681       0.806         0.2060 0.839   0.596
#> 5 5 0.662           0.624       0.769         0.0810 0.854   0.546
#> 6 6 0.775           0.808       0.862         0.0529 0.893   0.574
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.948 1.000 0.000
#&gt; GSM38713     1   0.204      0.952 0.968 0.032
#&gt; GSM38714     1   0.204      0.952 0.968 0.032
#&gt; GSM38715     1   0.204      0.952 0.968 0.032
#&gt; GSM38716     1   0.000      0.948 1.000 0.000
#&gt; GSM38717     1   0.000      0.948 1.000 0.000
#&gt; GSM38718     1   0.204      0.952 0.968 0.032
#&gt; GSM38719     1   0.000      0.948 1.000 0.000
#&gt; GSM38720     1   0.000      0.948 1.000 0.000
#&gt; GSM38721     1   0.204      0.952 0.968 0.032
#&gt; GSM38722     1   0.000      0.948 1.000 0.000
#&gt; GSM38723     1   0.000      0.948 1.000 0.000
#&gt; GSM38724     1   0.358      0.941 0.932 0.068
#&gt; GSM38725     1   0.000      0.948 1.000 0.000
#&gt; GSM38726     1   0.000      0.948 1.000 0.000
#&gt; GSM38727     1   0.000      0.948 1.000 0.000
#&gt; GSM38728     1   0.358      0.941 0.932 0.068
#&gt; GSM38729     1   0.680      0.735 0.820 0.180
#&gt; GSM38730     1   0.000      0.948 1.000 0.000
#&gt; GSM38731     1   0.000      0.948 1.000 0.000
#&gt; GSM38732     1   0.358      0.941 0.932 0.068
#&gt; GSM38733     1   0.204      0.952 0.968 0.032
#&gt; GSM38734     2   0.895      0.607 0.312 0.688
#&gt; GSM38735     2   0.000      0.890 0.000 1.000
#&gt; GSM38736     2   0.000      0.890 0.000 1.000
#&gt; GSM38737     2   0.000      0.890 0.000 1.000
#&gt; GSM38738     1   0.358      0.941 0.932 0.068
#&gt; GSM38739     1   0.204      0.950 0.968 0.032
#&gt; GSM38740     2   0.000      0.890 0.000 1.000
#&gt; GSM38741     1   0.925      0.463 0.660 0.340
#&gt; GSM38742     2   0.000      0.890 0.000 1.000
#&gt; GSM38743     2   0.000      0.890 0.000 1.000
#&gt; GSM38744     2   0.000      0.890 0.000 1.000
#&gt; GSM38745     2   0.000      0.890 0.000 1.000
#&gt; GSM38746     1   0.358      0.941 0.932 0.068
#&gt; GSM38747     1   0.358      0.941 0.932 0.068
#&gt; GSM38748     2   0.808      0.695 0.248 0.752
#&gt; GSM38749     1   0.163      0.951 0.976 0.024
#&gt; GSM38750     1   0.358      0.941 0.932 0.068
#&gt; GSM38751     1   0.358      0.941 0.932 0.068
#&gt; GSM38752     2   0.891      0.614 0.308 0.692
#&gt; GSM38753     2   0.260      0.872 0.044 0.956
#&gt; GSM38754     2   0.891      0.614 0.308 0.692
#&gt; GSM38755     1   0.358      0.941 0.932 0.068
#&gt; GSM38756     2   0.260      0.872 0.044 0.956
#&gt; GSM38757     1   0.358      0.941 0.932 0.068
#&gt; GSM38758     2   0.000      0.890 0.000 1.000
#&gt; GSM38759     2   0.975      0.380 0.408 0.592
#&gt; GSM38760     1   0.184      0.952 0.972 0.028
#&gt; GSM38761     2   0.000      0.890 0.000 1.000
#&gt; GSM38762     2   0.000      0.890 0.000 1.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38713     1  0.6079     0.2541 0.612 0.000 0.388
#&gt; GSM38714     1  0.6079     0.2541 0.612 0.000 0.388
#&gt; GSM38715     1  0.6079     0.2541 0.612 0.000 0.388
#&gt; GSM38716     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38717     1  0.0892     0.6673 0.980 0.000 0.020
#&gt; GSM38718     1  0.6062     0.2605 0.616 0.000 0.384
#&gt; GSM38719     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38721     1  0.6062     0.2605 0.616 0.000 0.384
#&gt; GSM38722     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38724     1  0.6291    -0.0793 0.532 0.000 0.468
#&gt; GSM38725     1  0.0892     0.6673 0.980 0.000 0.020
#&gt; GSM38726     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38728     1  0.6302    -0.0932 0.520 0.000 0.480
#&gt; GSM38729     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000     0.6745 1.000 0.000 0.000
#&gt; GSM38732     3  0.6267     0.3100 0.452 0.000 0.548
#&gt; GSM38733     1  0.6062     0.2605 0.616 0.000 0.384
#&gt; GSM38734     3  0.1765     0.5812 0.040 0.004 0.956
#&gt; GSM38735     2  0.0000     0.9967 0.000 1.000 0.000
#&gt; GSM38736     2  0.0237     0.9976 0.000 0.996 0.004
#&gt; GSM38737     2  0.0237     0.9976 0.000 0.996 0.004
#&gt; GSM38738     3  0.6267     0.3100 0.452 0.000 0.548
#&gt; GSM38739     1  0.6192     0.0596 0.580 0.000 0.420
#&gt; GSM38740     2  0.0000     0.9967 0.000 1.000 0.000
#&gt; GSM38741     3  0.3272     0.5739 0.104 0.004 0.892
#&gt; GSM38742     2  0.0237     0.9976 0.000 0.996 0.004
#&gt; GSM38743     2  0.0237     0.9976 0.000 0.996 0.004
#&gt; GSM38744     2  0.0000     0.9967 0.000 1.000 0.000
#&gt; GSM38745     2  0.0000     0.9967 0.000 1.000 0.000
#&gt; GSM38746     3  0.6314     0.4267 0.392 0.004 0.604
#&gt; GSM38747     3  0.6379     0.4532 0.368 0.008 0.624
#&gt; GSM38748     3  0.0661     0.5799 0.008 0.004 0.988
#&gt; GSM38749     1  0.6410     0.0504 0.576 0.004 0.420
#&gt; GSM38750     3  0.6410     0.3804 0.420 0.004 0.576
#&gt; GSM38751     3  0.6228     0.4501 0.372 0.004 0.624
#&gt; GSM38752     3  0.0237     0.5774 0.000 0.004 0.996
#&gt; GSM38753     3  0.0592     0.5765 0.000 0.012 0.988
#&gt; GSM38754     3  0.0237     0.5774 0.000 0.004 0.996
#&gt; GSM38755     3  0.6286     0.2810 0.464 0.000 0.536
#&gt; GSM38756     3  0.0592     0.5761 0.000 0.012 0.988
#&gt; GSM38757     3  0.6286     0.2810 0.464 0.000 0.536
#&gt; GSM38758     2  0.0747     0.9898 0.000 0.984 0.016
#&gt; GSM38759     3  0.6309     0.0581 0.500 0.000 0.500
#&gt; GSM38760     1  0.6286    -0.0668 0.536 0.000 0.464
#&gt; GSM38761     2  0.0237     0.9976 0.000 0.996 0.004
#&gt; GSM38762     2  0.0237     0.9976 0.000 0.996 0.004
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0188      0.793 0.996 0.000 0.004 0.000
#&gt; GSM38713     1  0.5917      0.539 0.624 0.000 0.320 0.056
#&gt; GSM38714     1  0.6570      0.492 0.580 0.000 0.320 0.100
#&gt; GSM38715     1  0.5130      0.572 0.668 0.000 0.312 0.020
#&gt; GSM38716     1  0.0188      0.793 0.996 0.000 0.004 0.000
#&gt; GSM38717     1  0.0524      0.793 0.988 0.000 0.004 0.008
#&gt; GSM38718     1  0.3266      0.739 0.868 0.000 0.108 0.024
#&gt; GSM38719     1  0.0336      0.793 0.992 0.000 0.000 0.008
#&gt; GSM38720     1  0.0672      0.792 0.984 0.000 0.008 0.008
#&gt; GSM38721     1  0.6280      0.516 0.612 0.000 0.304 0.084
#&gt; GSM38722     1  0.1022      0.774 0.968 0.000 0.032 0.000
#&gt; GSM38723     1  0.1118      0.771 0.964 0.000 0.036 0.000
#&gt; GSM38724     3  0.5426      0.416 0.232 0.000 0.708 0.060
#&gt; GSM38725     1  0.0469      0.791 0.988 0.000 0.012 0.000
#&gt; GSM38726     1  0.0188      0.793 0.996 0.000 0.004 0.000
#&gt; GSM38727     1  0.1716      0.742 0.936 0.000 0.064 0.000
#&gt; GSM38728     1  0.6960      0.316 0.468 0.000 0.420 0.112
#&gt; GSM38729     1  0.0672      0.792 0.984 0.000 0.008 0.008
#&gt; GSM38730     1  0.0188      0.793 0.996 0.000 0.004 0.000
#&gt; GSM38731     1  0.0188      0.793 0.996 0.000 0.004 0.000
#&gt; GSM38732     3  0.7248      0.521 0.148 0.000 0.472 0.380
#&gt; GSM38733     1  0.6810      0.467 0.596 0.000 0.248 0.156
#&gt; GSM38734     4  0.3392      0.466 0.020 0.000 0.124 0.856
#&gt; GSM38735     2  0.0336      0.971 0.000 0.992 0.000 0.008
#&gt; GSM38736     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38738     3  0.7113      0.567 0.152 0.000 0.532 0.316
#&gt; GSM38739     3  0.4916      0.547 0.424 0.000 0.576 0.000
#&gt; GSM38740     2  0.0336      0.971 0.000 0.992 0.000 0.008
#&gt; GSM38741     3  0.2919      0.349 0.044 0.000 0.896 0.060
#&gt; GSM38742     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38743     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38744     2  0.0336      0.971 0.000 0.992 0.000 0.008
#&gt; GSM38745     2  0.0336      0.971 0.000 0.992 0.000 0.008
#&gt; GSM38746     3  0.4331      0.557 0.288 0.000 0.712 0.000
#&gt; GSM38747     3  0.3320      0.321 0.068 0.000 0.876 0.056
#&gt; GSM38748     4  0.1118      0.562 0.000 0.000 0.036 0.964
#&gt; GSM38749     3  0.4916      0.547 0.424 0.000 0.576 0.000
#&gt; GSM38750     3  0.6476      0.574 0.112 0.000 0.616 0.272
#&gt; GSM38751     3  0.4936      0.574 0.316 0.000 0.672 0.012
#&gt; GSM38752     4  0.4855      0.571 0.000 0.000 0.400 0.600
#&gt; GSM38753     4  0.4869      0.544 0.260 0.004 0.016 0.720
#&gt; GSM38754     4  0.4855      0.571 0.000 0.000 0.400 0.600
#&gt; GSM38755     3  0.7149      0.566 0.156 0.000 0.528 0.316
#&gt; GSM38756     4  0.4869      0.544 0.260 0.004 0.016 0.720
#&gt; GSM38757     3  0.7159      0.596 0.180 0.000 0.548 0.272
#&gt; GSM38758     2  0.3907      0.700 0.000 0.768 0.000 0.232
#&gt; GSM38759     1  0.5772      0.595 0.708 0.000 0.176 0.116
#&gt; GSM38760     3  0.5292      0.468 0.480 0.000 0.512 0.008
#&gt; GSM38761     2  0.0188      0.971 0.000 0.996 0.000 0.004
#&gt; GSM38762     2  0.0000      0.972 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0963     0.7460 0.964 0.000 0.036 0.000 0.000
#&gt; GSM38713     5  0.4958     0.6936 0.372 0.000 0.036 0.000 0.592
#&gt; GSM38714     5  0.5392     0.7259 0.252 0.000 0.056 0.024 0.668
#&gt; GSM38715     5  0.5262     0.6765 0.388 0.000 0.036 0.008 0.568
#&gt; GSM38716     1  0.0703     0.7314 0.976 0.000 0.000 0.000 0.024
#&gt; GSM38717     1  0.3282     0.6304 0.804 0.000 0.008 0.000 0.188
#&gt; GSM38718     1  0.6930    -0.0715 0.488 0.000 0.072 0.084 0.356
#&gt; GSM38719     1  0.2806     0.6608 0.844 0.000 0.004 0.000 0.152
#&gt; GSM38720     1  0.3715     0.5625 0.736 0.000 0.004 0.000 0.260
#&gt; GSM38721     5  0.4853     0.7353 0.296 0.000 0.032 0.008 0.664
#&gt; GSM38722     1  0.1043     0.7458 0.960 0.000 0.040 0.000 0.000
#&gt; GSM38723     1  0.1043     0.7458 0.960 0.000 0.040 0.000 0.000
#&gt; GSM38724     3  0.5454     0.4503 0.036 0.000 0.608 0.024 0.332
#&gt; GSM38725     1  0.1043     0.7459 0.960 0.000 0.040 0.000 0.000
#&gt; GSM38726     1  0.0162     0.7445 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38727     1  0.0290     0.7444 0.992 0.000 0.008 0.000 0.000
#&gt; GSM38728     5  0.6521    -0.1479 0.024 0.000 0.388 0.108 0.480
#&gt; GSM38729     1  0.3452     0.5761 0.756 0.000 0.000 0.000 0.244
#&gt; GSM38730     1  0.0963     0.7454 0.964 0.000 0.036 0.000 0.000
#&gt; GSM38731     1  0.0451     0.7413 0.988 0.000 0.004 0.000 0.008
#&gt; GSM38732     3  0.3312     0.6245 0.048 0.000 0.864 0.020 0.068
#&gt; GSM38733     5  0.5519     0.7258 0.292 0.000 0.076 0.008 0.624
#&gt; GSM38734     4  0.4558     0.6460 0.000 0.000 0.324 0.652 0.024
#&gt; GSM38735     2  0.3395     0.7985 0.000 0.764 0.000 0.000 0.236
#&gt; GSM38736     2  0.1043     0.8718 0.000 0.960 0.000 0.040 0.000
#&gt; GSM38737     2  0.1043     0.8718 0.000 0.960 0.000 0.040 0.000
#&gt; GSM38738     3  0.2885     0.6334 0.052 0.000 0.880 0.004 0.064
#&gt; GSM38739     1  0.4302     0.0803 0.520 0.000 0.480 0.000 0.000
#&gt; GSM38740     2  0.3395     0.7985 0.000 0.764 0.000 0.000 0.236
#&gt; GSM38741     3  0.5760     0.4787 0.008 0.000 0.620 0.108 0.264
#&gt; GSM38742     2  0.1270     0.8669 0.000 0.948 0.000 0.052 0.000
#&gt; GSM38743     2  0.1043     0.8718 0.000 0.960 0.000 0.040 0.000
#&gt; GSM38744     2  0.3395     0.7985 0.000 0.764 0.000 0.000 0.236
#&gt; GSM38745     2  0.3395     0.7985 0.000 0.764 0.000 0.000 0.236
#&gt; GSM38746     3  0.5373     0.4683 0.296 0.000 0.620 0.000 0.084
#&gt; GSM38747     3  0.5515     0.4749 0.000 0.000 0.624 0.108 0.268
#&gt; GSM38748     4  0.3895     0.6602 0.000 0.000 0.320 0.680 0.000
#&gt; GSM38749     1  0.4192     0.3362 0.596 0.000 0.404 0.000 0.000
#&gt; GSM38750     3  0.1357     0.6463 0.048 0.000 0.948 0.000 0.004
#&gt; GSM38751     3  0.4503     0.4448 0.312 0.000 0.664 0.000 0.024
#&gt; GSM38752     4  0.4622     0.5734 0.000 0.000 0.044 0.692 0.264
#&gt; GSM38753     4  0.2685     0.7096 0.092 0.000 0.028 0.880 0.000
#&gt; GSM38754     4  0.4264     0.6198 0.000 0.000 0.044 0.744 0.212
#&gt; GSM38755     3  0.3019     0.6316 0.088 0.000 0.864 0.000 0.048
#&gt; GSM38756     4  0.2685     0.7096 0.092 0.000 0.028 0.880 0.000
#&gt; GSM38757     3  0.1800     0.6486 0.048 0.000 0.932 0.000 0.020
#&gt; GSM38758     2  0.3109     0.7227 0.000 0.800 0.000 0.200 0.000
#&gt; GSM38759     3  0.8134    -0.0348 0.228 0.000 0.344 0.108 0.320
#&gt; GSM38760     1  0.4482     0.4036 0.636 0.000 0.348 0.000 0.016
#&gt; GSM38761     2  0.1197     0.8689 0.000 0.952 0.000 0.048 0.000
#&gt; GSM38762     2  0.1043     0.8718 0.000 0.960 0.000 0.040 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.3126      0.745 0.752 0.000 0.000 0.000 0.000 0.248
#&gt; GSM38713     6  0.3193      0.815 0.052 0.000 0.124 0.000 0.000 0.824
#&gt; GSM38714     6  0.2791      0.791 0.032 0.000 0.096 0.000 0.008 0.864
#&gt; GSM38715     6  0.3193      0.815 0.052 0.000 0.124 0.000 0.000 0.824
#&gt; GSM38716     1  0.2527      0.790 0.832 0.000 0.000 0.000 0.000 0.168
#&gt; GSM38717     1  0.4203      0.591 0.596 0.000 0.008 0.000 0.008 0.388
#&gt; GSM38718     6  0.5331      0.234 0.320 0.000 0.088 0.004 0.008 0.580
#&gt; GSM38719     1  0.3874      0.649 0.636 0.000 0.000 0.000 0.008 0.356
#&gt; GSM38720     1  0.3887      0.646 0.632 0.000 0.000 0.000 0.008 0.360
#&gt; GSM38721     6  0.3130      0.815 0.048 0.000 0.124 0.000 0.000 0.828
#&gt; GSM38722     1  0.0000      0.770 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.770 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.1970      0.804 0.008 0.000 0.900 0.000 0.000 0.092
#&gt; GSM38725     1  0.1913      0.784 0.908 0.000 0.012 0.000 0.000 0.080
#&gt; GSM38726     1  0.1204      0.795 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM38727     1  0.0000      0.770 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38728     6  0.3819      0.531 0.012 0.000 0.316 0.000 0.000 0.672
#&gt; GSM38729     1  0.3887      0.646 0.632 0.000 0.000 0.000 0.008 0.360
#&gt; GSM38730     1  0.1765      0.804 0.904 0.000 0.000 0.000 0.000 0.096
#&gt; GSM38731     1  0.2003      0.803 0.884 0.000 0.000 0.000 0.000 0.116
#&gt; GSM38732     3  0.0865      0.817 0.000 0.000 0.964 0.000 0.000 0.036
#&gt; GSM38733     6  0.3130      0.815 0.048 0.000 0.124 0.000 0.000 0.828
#&gt; GSM38734     4  0.0713      0.969 0.000 0.000 0.028 0.972 0.000 0.000
#&gt; GSM38735     5  0.1141      1.000 0.000 0.052 0.000 0.000 0.948 0.000
#&gt; GSM38736     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38737     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38738     3  0.0363      0.823 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; GSM38739     3  0.4967      0.632 0.200 0.000 0.692 0.000 0.044 0.064
#&gt; GSM38740     5  0.1141      1.000 0.000 0.052 0.000 0.000 0.948 0.000
#&gt; GSM38741     3  0.3062      0.766 0.000 0.000 0.836 0.052 0.000 0.112
#&gt; GSM38742     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38744     5  0.1141      1.000 0.000 0.052 0.000 0.000 0.948 0.000
#&gt; GSM38745     5  0.1141      1.000 0.000 0.052 0.000 0.000 0.948 0.000
#&gt; GSM38746     3  0.2852      0.805 0.020 0.000 0.872 0.000 0.044 0.064
#&gt; GSM38747     3  0.3808      0.599 0.012 0.000 0.700 0.004 0.000 0.284
#&gt; GSM38748     4  0.0363      0.976 0.000 0.000 0.012 0.988 0.000 0.000
#&gt; GSM38749     3  0.5022      0.621 0.208 0.000 0.684 0.000 0.044 0.064
#&gt; GSM38750     3  0.0146      0.824 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM38751     3  0.2259      0.814 0.008 0.000 0.904 0.000 0.044 0.044
#&gt; GSM38752     4  0.1141      0.966 0.000 0.000 0.000 0.948 0.000 0.052
#&gt; GSM38753     4  0.0146      0.977 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM38754     4  0.1075      0.967 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; GSM38755     3  0.0713      0.820 0.000 0.000 0.972 0.000 0.000 0.028
#&gt; GSM38756     4  0.0146      0.977 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM38757     3  0.0363      0.823 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; GSM38758     2  0.1007      0.947 0.000 0.956 0.000 0.044 0.000 0.000
#&gt; GSM38759     6  0.4259      0.557 0.020 0.000 0.292 0.004 0.008 0.676
#&gt; GSM38760     3  0.5554      0.430 0.316 0.000 0.576 0.000 0.044 0.064
#&gt; GSM38761     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.004 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n individual(p) k
#> SD:mclust 49      0.018116 2
#> SD:mclust 31      0.004521 3
#> SD:mclust 43      0.000760 4
#> SD:mclust 40      0.005273 5
#> SD:mclust 49      0.000791 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.978       0.990         0.4154 0.594   0.594
#> 3 3 1.000           0.978       0.990         0.3965 0.749   0.603
#> 4 4 0.830           0.855       0.923         0.2735 0.788   0.522
#> 5 5 0.814           0.779       0.893         0.0416 0.940   0.779
#> 6 6 0.708           0.666       0.832         0.0343 0.944   0.784
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.986 1.000 0.000
#&gt; GSM38713     1  0.0000      0.986 1.000 0.000
#&gt; GSM38714     1  0.0000      0.986 1.000 0.000
#&gt; GSM38715     1  0.0000      0.986 1.000 0.000
#&gt; GSM38716     1  0.0000      0.986 1.000 0.000
#&gt; GSM38717     1  0.0000      0.986 1.000 0.000
#&gt; GSM38718     1  0.0000      0.986 1.000 0.000
#&gt; GSM38719     1  0.0000      0.986 1.000 0.000
#&gt; GSM38720     1  0.0000      0.986 1.000 0.000
#&gt; GSM38721     1  0.0000      0.986 1.000 0.000
#&gt; GSM38722     1  0.0000      0.986 1.000 0.000
#&gt; GSM38723     1  0.0000      0.986 1.000 0.000
#&gt; GSM38724     1  0.0000      0.986 1.000 0.000
#&gt; GSM38725     1  0.0000      0.986 1.000 0.000
#&gt; GSM38726     1  0.0000      0.986 1.000 0.000
#&gt; GSM38727     1  0.0000      0.986 1.000 0.000
#&gt; GSM38728     1  0.6712      0.791 0.824 0.176
#&gt; GSM38729     1  0.0000      0.986 1.000 0.000
#&gt; GSM38730     1  0.0000      0.986 1.000 0.000
#&gt; GSM38731     1  0.0000      0.986 1.000 0.000
#&gt; GSM38732     1  0.0000      0.986 1.000 0.000
#&gt; GSM38733     1  0.0000      0.986 1.000 0.000
#&gt; GSM38734     2  0.0000      1.000 0.000 1.000
#&gt; GSM38735     1  0.8661      0.605 0.712 0.288
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000
#&gt; GSM38738     1  0.0376      0.983 0.996 0.004
#&gt; GSM38739     1  0.0000      0.986 1.000 0.000
#&gt; GSM38740     1  0.0000      0.986 1.000 0.000
#&gt; GSM38741     2  0.0000      1.000 0.000 1.000
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000
#&gt; GSM38744     1  0.0000      0.986 1.000 0.000
#&gt; GSM38745     1  0.0000      0.986 1.000 0.000
#&gt; GSM38746     1  0.0000      0.986 1.000 0.000
#&gt; GSM38747     1  0.2423      0.950 0.960 0.040
#&gt; GSM38748     2  0.0000      1.000 0.000 1.000
#&gt; GSM38749     1  0.0000      0.986 1.000 0.000
#&gt; GSM38750     1  0.0000      0.986 1.000 0.000
#&gt; GSM38751     1  0.0376      0.983 0.996 0.004
#&gt; GSM38752     2  0.0000      1.000 0.000 1.000
#&gt; GSM38753     2  0.0000      1.000 0.000 1.000
#&gt; GSM38754     2  0.0000      1.000 0.000 1.000
#&gt; GSM38755     1  0.0000      0.986 1.000 0.000
#&gt; GSM38756     2  0.0000      1.000 0.000 1.000
#&gt; GSM38757     1  0.0000      0.986 1.000 0.000
#&gt; GSM38758     2  0.0000      1.000 0.000 1.000
#&gt; GSM38759     1  0.0000      0.986 1.000 0.000
#&gt; GSM38760     1  0.0000      0.986 1.000 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38721     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38722     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38724     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38725     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38728     3  0.3192      0.861 0.112 0.000 0.888
#&gt; GSM38729     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38732     3  0.1860      0.928 0.052 0.000 0.948
#&gt; GSM38733     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38734     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38735     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM38736     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM38738     3  0.2625      0.898 0.084 0.000 0.916
#&gt; GSM38739     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38740     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM38741     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38742     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM38744     2  0.0237      0.994 0.004 0.996 0.000
#&gt; GSM38745     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM38746     1  0.0424      0.985 0.992 0.000 0.008
#&gt; GSM38747     1  0.4062      0.802 0.836 0.000 0.164
#&gt; GSM38748     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38749     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38750     1  0.0592      0.982 0.988 0.000 0.012
#&gt; GSM38751     1  0.0237      0.988 0.996 0.000 0.004
#&gt; GSM38752     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38753     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38754     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38755     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38756     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38757     1  0.1529      0.957 0.960 0.000 0.040
#&gt; GSM38758     2  0.0237      0.995 0.000 0.996 0.004
#&gt; GSM38759     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM38760     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM38761     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000      0.999 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.1557      0.904 0.944 0.000 0.056 0.000
#&gt; GSM38713     1  0.0707      0.912 0.980 0.000 0.020 0.000
#&gt; GSM38714     1  0.0188      0.924 0.996 0.000 0.004 0.000
#&gt; GSM38715     1  0.0000      0.922 1.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0817      0.919 0.976 0.000 0.024 0.000
#&gt; GSM38717     1  0.0336      0.924 0.992 0.000 0.008 0.000
#&gt; GSM38718     1  0.0336      0.924 0.992 0.000 0.008 0.000
#&gt; GSM38719     1  0.0336      0.924 0.992 0.000 0.008 0.000
#&gt; GSM38720     1  0.0336      0.924 0.992 0.000 0.008 0.000
#&gt; GSM38721     1  0.0188      0.924 0.996 0.000 0.004 0.000
#&gt; GSM38722     3  0.2921      0.829 0.140 0.000 0.860 0.000
#&gt; GSM38723     3  0.1940      0.876 0.076 0.000 0.924 0.000
#&gt; GSM38724     3  0.7876      0.128 0.352 0.000 0.368 0.280
#&gt; GSM38725     1  0.4222      0.632 0.728 0.000 0.272 0.000
#&gt; GSM38726     1  0.3024      0.820 0.852 0.000 0.148 0.000
#&gt; GSM38727     3  0.2011      0.874 0.080 0.000 0.920 0.000
#&gt; GSM38728     1  0.4800      0.481 0.656 0.000 0.004 0.340
#&gt; GSM38729     1  0.0336      0.924 0.992 0.000 0.008 0.000
#&gt; GSM38730     1  0.3219      0.800 0.836 0.000 0.164 0.000
#&gt; GSM38731     1  0.1940      0.888 0.924 0.000 0.076 0.000
#&gt; GSM38732     4  0.5596      0.664 0.236 0.000 0.068 0.696
#&gt; GSM38733     1  0.1211      0.896 0.960 0.000 0.040 0.000
#&gt; GSM38734     4  0.2048      0.916 0.008 0.000 0.064 0.928
#&gt; GSM38735     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM38736     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM38738     4  0.2593      0.894 0.004 0.000 0.104 0.892
#&gt; GSM38739     3  0.1716      0.879 0.064 0.000 0.936 0.000
#&gt; GSM38740     3  0.4964      0.348 0.004 0.380 0.616 0.000
#&gt; GSM38741     4  0.0336      0.940 0.000 0.000 0.008 0.992
#&gt; GSM38742     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM38743     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM38744     2  0.4599      0.618 0.016 0.736 0.248 0.000
#&gt; GSM38745     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM38746     3  0.1824      0.878 0.060 0.000 0.936 0.004
#&gt; GSM38747     3  0.2926      0.856 0.048 0.000 0.896 0.056
#&gt; GSM38748     4  0.0376      0.942 0.004 0.000 0.004 0.992
#&gt; GSM38749     3  0.1716      0.879 0.064 0.000 0.936 0.000
#&gt; GSM38750     3  0.0188      0.846 0.004 0.000 0.996 0.000
#&gt; GSM38751     3  0.1637      0.878 0.060 0.000 0.940 0.000
#&gt; GSM38752     4  0.0000      0.943 0.000 0.000 0.000 1.000
#&gt; GSM38753     4  0.0000      0.943 0.000 0.000 0.000 1.000
#&gt; GSM38754     4  0.0000      0.943 0.000 0.000 0.000 1.000
#&gt; GSM38755     3  0.0000      0.843 0.000 0.000 1.000 0.000
#&gt; GSM38756     4  0.0000      0.943 0.000 0.000 0.000 1.000
#&gt; GSM38757     3  0.3668      0.671 0.004 0.000 0.808 0.188
#&gt; GSM38758     2  0.1022      0.937 0.000 0.968 0.000 0.032
#&gt; GSM38759     1  0.1042      0.916 0.972 0.000 0.008 0.020
#&gt; GSM38760     3  0.1792      0.878 0.068 0.000 0.932 0.000
#&gt; GSM38761     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM38762     2  0.0000      0.963 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0510      0.896 0.984 0.000 0.016 0.000 0.000
#&gt; GSM38713     1  0.2891      0.782 0.824 0.000 0.000 0.000 0.176
#&gt; GSM38714     1  0.0510      0.897 0.984 0.000 0.000 0.000 0.016
#&gt; GSM38715     1  0.1341      0.879 0.944 0.000 0.000 0.000 0.056
#&gt; GSM38716     1  0.0162      0.899 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38717     1  0.0162      0.899 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38718     1  0.0290      0.899 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38719     1  0.0404      0.898 0.988 0.000 0.000 0.000 0.012
#&gt; GSM38720     1  0.0451      0.899 0.988 0.000 0.004 0.000 0.008
#&gt; GSM38721     1  0.0162      0.899 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38722     3  0.4249      0.253 0.432 0.000 0.568 0.000 0.000
#&gt; GSM38723     3  0.0794      0.769 0.028 0.000 0.972 0.000 0.000
#&gt; GSM38724     1  0.5191      0.597 0.684 0.000 0.192 0.124 0.000
#&gt; GSM38725     1  0.3586      0.622 0.736 0.000 0.264 0.000 0.000
#&gt; GSM38726     1  0.1205      0.887 0.956 0.000 0.040 0.000 0.004
#&gt; GSM38727     3  0.2074      0.692 0.104 0.000 0.896 0.000 0.000
#&gt; GSM38728     1  0.5263      0.378 0.576 0.000 0.000 0.368 0.056
#&gt; GSM38729     1  0.0162      0.899 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38730     1  0.0703      0.893 0.976 0.000 0.024 0.000 0.000
#&gt; GSM38731     1  0.0324      0.899 0.992 0.000 0.004 0.000 0.004
#&gt; GSM38732     5  0.2390      0.587 0.000 0.000 0.020 0.084 0.896
#&gt; GSM38733     1  0.3949      0.585 0.668 0.000 0.000 0.000 0.332
#&gt; GSM38734     5  0.2424      0.549 0.000 0.000 0.000 0.132 0.868
#&gt; GSM38735     2  0.1792      0.876 0.000 0.916 0.000 0.000 0.084
#&gt; GSM38736     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38738     5  0.5086      0.664 0.000 0.000 0.304 0.060 0.636
#&gt; GSM38739     3  0.0000      0.781 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38740     3  0.7679      0.233 0.268 0.192 0.456 0.000 0.084
#&gt; GSM38741     4  0.0162      0.973 0.000 0.000 0.004 0.996 0.000
#&gt; GSM38742     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     2  0.6791      0.402 0.292 0.548 0.076 0.000 0.084
#&gt; GSM38745     2  0.1952      0.874 0.004 0.912 0.000 0.000 0.084
#&gt; GSM38746     3  0.0000      0.781 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38747     3  0.2763      0.635 0.004 0.000 0.848 0.148 0.000
#&gt; GSM38748     4  0.2074      0.878 0.000 0.000 0.000 0.896 0.104
#&gt; GSM38749     3  0.0000      0.781 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38750     3  0.0880      0.749 0.000 0.000 0.968 0.000 0.032
#&gt; GSM38751     3  0.0000      0.781 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38752     4  0.0000      0.976 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38753     4  0.0000      0.976 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38754     4  0.0000      0.976 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38755     5  0.4256      0.561 0.000 0.000 0.436 0.000 0.564
#&gt; GSM38756     4  0.0000      0.976 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38757     5  0.4305      0.471 0.000 0.000 0.488 0.000 0.512
#&gt; GSM38758     2  0.2179      0.822 0.000 0.888 0.000 0.112 0.000
#&gt; GSM38759     1  0.1608      0.866 0.928 0.000 0.000 0.072 0.000
#&gt; GSM38760     3  0.0000      0.781 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38761     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1010     0.7310 0.960 0.000 0.004 0.000 0.036 0.000
#&gt; GSM38713     1  0.3221     0.6889 0.828 0.000 0.000 0.000 0.096 0.076
#&gt; GSM38714     1  0.1075     0.7318 0.952 0.000 0.000 0.000 0.048 0.000
#&gt; GSM38715     1  0.1616     0.7276 0.932 0.000 0.000 0.000 0.048 0.020
#&gt; GSM38716     1  0.1003     0.7338 0.964 0.000 0.016 0.000 0.020 0.000
#&gt; GSM38717     1  0.0632     0.7340 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM38718     1  0.1863     0.7133 0.896 0.000 0.000 0.000 0.104 0.000
#&gt; GSM38719     1  0.0458     0.7365 0.984 0.000 0.000 0.000 0.016 0.000
#&gt; GSM38720     1  0.1444     0.7288 0.928 0.000 0.000 0.000 0.072 0.000
#&gt; GSM38721     1  0.1075     0.7292 0.952 0.000 0.000 0.000 0.048 0.000
#&gt; GSM38722     1  0.4335     0.0550 0.508 0.000 0.472 0.000 0.020 0.000
#&gt; GSM38723     3  0.2070     0.7557 0.100 0.000 0.892 0.000 0.008 0.000
#&gt; GSM38724     1  0.6396     0.1997 0.580 0.000 0.092 0.148 0.176 0.004
#&gt; GSM38725     1  0.5852    -0.0608 0.544 0.000 0.192 0.000 0.252 0.012
#&gt; GSM38726     1  0.3756     0.5077 0.712 0.000 0.268 0.000 0.020 0.000
#&gt; GSM38727     3  0.2704     0.7095 0.140 0.000 0.844 0.000 0.016 0.000
#&gt; GSM38728     1  0.5874     0.2862 0.500 0.000 0.000 0.292 0.204 0.004
#&gt; GSM38729     1  0.1501     0.7312 0.924 0.000 0.000 0.000 0.076 0.000
#&gt; GSM38730     1  0.1418     0.7269 0.944 0.000 0.024 0.000 0.032 0.000
#&gt; GSM38731     1  0.4175     0.6028 0.740 0.000 0.156 0.000 0.104 0.000
#&gt; GSM38732     6  0.2454     0.7910 0.000 0.000 0.000 0.000 0.160 0.840
#&gt; GSM38733     1  0.5851     0.2650 0.484 0.000 0.000 0.000 0.280 0.236
#&gt; GSM38734     6  0.0291     0.8169 0.000 0.000 0.000 0.004 0.004 0.992
#&gt; GSM38735     2  0.2730     0.7309 0.000 0.808 0.000 0.000 0.192 0.000
#&gt; GSM38736     2  0.0000     0.8987 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000     0.8987 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38738     6  0.3376     0.7656 0.000 0.000 0.128 0.004 0.052 0.816
#&gt; GSM38739     3  0.0777     0.7939 0.000 0.000 0.972 0.000 0.024 0.004
#&gt; GSM38740     5  0.6840     0.7420 0.240 0.080 0.208 0.000 0.472 0.000
#&gt; GSM38741     4  0.0405     0.8468 0.000 0.000 0.008 0.988 0.004 0.000
#&gt; GSM38742     2  0.0146     0.8975 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38743     2  0.0000     0.8987 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.6642     0.7354 0.256 0.220 0.052 0.000 0.472 0.000
#&gt; GSM38745     2  0.4099     0.3656 0.000 0.612 0.016 0.000 0.372 0.000
#&gt; GSM38746     3  0.2941     0.6264 0.000 0.000 0.780 0.000 0.220 0.000
#&gt; GSM38747     3  0.5307     0.4920 0.020 0.000 0.664 0.144 0.168 0.004
#&gt; GSM38748     4  0.5657     0.0730 0.000 0.000 0.000 0.436 0.152 0.412
#&gt; GSM38749     3  0.0551     0.7984 0.008 0.000 0.984 0.000 0.004 0.004
#&gt; GSM38750     3  0.1812     0.7829 0.000 0.000 0.912 0.000 0.008 0.080
#&gt; GSM38751     3  0.0363     0.7970 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM38752     4  0.1462     0.8411 0.000 0.000 0.000 0.936 0.008 0.056
#&gt; GSM38753     4  0.0520     0.8488 0.000 0.000 0.000 0.984 0.008 0.008
#&gt; GSM38754     4  0.2039     0.8282 0.000 0.000 0.000 0.904 0.020 0.076
#&gt; GSM38755     3  0.4434     0.2155 0.000 0.000 0.544 0.000 0.028 0.428
#&gt; GSM38756     4  0.0858     0.8452 0.000 0.000 0.000 0.968 0.004 0.028
#&gt; GSM38757     3  0.3512     0.6623 0.000 0.000 0.772 0.000 0.032 0.196
#&gt; GSM38758     2  0.2020     0.8084 0.000 0.896 0.000 0.096 0.008 0.000
#&gt; GSM38759     1  0.4011     0.5636 0.736 0.000 0.000 0.204 0.060 0.000
#&gt; GSM38760     3  0.1471     0.7824 0.064 0.000 0.932 0.000 0.004 0.000
#&gt; GSM38761     2  0.0146     0.8975 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38762     2  0.0000     0.8987 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n individual(p) k
#> SD:NMF 51      0.046137 2
#> SD:NMF 51      0.008714 3
#> SD:NMF 48      0.000723 4
#> SD:NMF 46      0.002081 5
#> SD:NMF 42      0.003813 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.361           0.661       0.827          0.370 0.730   0.730
#> 3 3 0.400           0.584       0.765          0.563 0.802   0.729
#> 4 4 0.551           0.641       0.745          0.118 0.869   0.765
#> 5 5 0.571           0.671       0.760          0.039 0.991   0.979
#> 6 6 0.761           0.813       0.868          0.164 0.787   0.518
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.764 1.000 0.000
#&gt; GSM38713     1  0.2778      0.765 0.952 0.048
#&gt; GSM38714     1  0.2778      0.765 0.952 0.048
#&gt; GSM38715     1  0.2778      0.765 0.952 0.048
#&gt; GSM38716     1  0.0000      0.764 1.000 0.000
#&gt; GSM38717     1  0.2423      0.767 0.960 0.040
#&gt; GSM38718     1  0.1843      0.766 0.972 0.028
#&gt; GSM38719     1  0.0000      0.764 1.000 0.000
#&gt; GSM38720     1  0.0000      0.764 1.000 0.000
#&gt; GSM38721     1  0.8909      0.639 0.692 0.308
#&gt; GSM38722     1  0.0000      0.764 1.000 0.000
#&gt; GSM38723     1  0.0000      0.764 1.000 0.000
#&gt; GSM38724     1  0.9286      0.610 0.656 0.344
#&gt; GSM38725     1  0.2236      0.766 0.964 0.036
#&gt; GSM38726     1  0.0000      0.764 1.000 0.000
#&gt; GSM38727     1  0.0000      0.764 1.000 0.000
#&gt; GSM38728     1  0.9983      0.421 0.524 0.476
#&gt; GSM38729     1  0.0000      0.764 1.000 0.000
#&gt; GSM38730     1  0.0000      0.764 1.000 0.000
#&gt; GSM38731     1  0.0000      0.764 1.000 0.000
#&gt; GSM38732     2  0.5294      0.773 0.120 0.880
#&gt; GSM38733     1  0.9661      0.531 0.608 0.392
#&gt; GSM38734     2  0.0376      0.880 0.004 0.996
#&gt; GSM38735     1  0.0376      0.762 0.996 0.004
#&gt; GSM38736     1  0.9993      0.396 0.516 0.484
#&gt; GSM38737     1  0.9993      0.396 0.516 0.484
#&gt; GSM38738     2  0.9963     -0.246 0.464 0.536
#&gt; GSM38739     1  0.6712      0.723 0.824 0.176
#&gt; GSM38740     1  0.0376      0.762 0.996 0.004
#&gt; GSM38741     1  0.9775      0.528 0.588 0.412
#&gt; GSM38742     1  0.9996      0.388 0.512 0.488
#&gt; GSM38743     1  0.9993      0.396 0.516 0.484
#&gt; GSM38744     1  0.0376      0.762 0.996 0.004
#&gt; GSM38745     1  0.0376      0.762 0.996 0.004
#&gt; GSM38746     1  0.9286      0.613 0.656 0.344
#&gt; GSM38747     1  0.9286      0.613 0.656 0.344
#&gt; GSM38748     2  0.0376      0.880 0.004 0.996
#&gt; GSM38749     1  0.6712      0.723 0.824 0.176
#&gt; GSM38750     1  0.8499      0.669 0.724 0.276
#&gt; GSM38751     1  0.8499      0.669 0.724 0.276
#&gt; GSM38752     2  0.0376      0.880 0.004 0.996
#&gt; GSM38753     2  0.1633      0.876 0.024 0.976
#&gt; GSM38754     2  0.0376      0.880 0.004 0.996
#&gt; GSM38755     1  0.9286      0.583 0.656 0.344
#&gt; GSM38756     2  0.1633      0.876 0.024 0.976
#&gt; GSM38757     1  1.0000      0.316 0.504 0.496
#&gt; GSM38758     1  0.9996      0.388 0.512 0.488
#&gt; GSM38759     1  0.1633      0.766 0.976 0.024
#&gt; GSM38760     1  0.5737      0.737 0.864 0.136
#&gt; GSM38761     1  0.9996      0.388 0.512 0.488
#&gt; GSM38762     1  0.9993      0.396 0.516 0.484
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38713     1  0.2031      0.695 0.952 0.032 0.016
#&gt; GSM38714     1  0.2031      0.695 0.952 0.032 0.016
#&gt; GSM38715     1  0.2031      0.695 0.952 0.032 0.016
#&gt; GSM38716     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38717     1  0.3879      0.668 0.848 0.152 0.000
#&gt; GSM38718     1  0.1163      0.696 0.972 0.028 0.000
#&gt; GSM38719     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38721     1  0.9042      0.481 0.544 0.280 0.176
#&gt; GSM38722     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38724     1  0.9553      0.421 0.484 0.244 0.272
#&gt; GSM38725     1  0.1525      0.696 0.964 0.032 0.004
#&gt; GSM38726     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38728     1  0.9947      0.196 0.376 0.288 0.336
#&gt; GSM38729     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.697 1.000 0.000 0.000
#&gt; GSM38732     3  0.7613      0.496 0.116 0.204 0.680
#&gt; GSM38733     1  0.9657      0.355 0.460 0.300 0.240
#&gt; GSM38734     3  0.5404      0.649 0.004 0.256 0.740
#&gt; GSM38735     1  0.7710      0.161 0.576 0.368 0.056
#&gt; GSM38736     2  0.0237      0.996 0.004 0.996 0.000
#&gt; GSM38737     2  0.0237      0.996 0.004 0.996 0.000
#&gt; GSM38738     3  0.9986     -0.131 0.340 0.308 0.352
#&gt; GSM38739     1  0.7966      0.580 0.652 0.220 0.128
#&gt; GSM38740     1  0.7710      0.161 0.576 0.368 0.056
#&gt; GSM38741     1  0.9736      0.306 0.416 0.228 0.356
#&gt; GSM38742     2  0.0475      0.994 0.004 0.992 0.004
#&gt; GSM38743     2  0.0237      0.996 0.004 0.996 0.000
#&gt; GSM38744     1  0.7710      0.161 0.576 0.368 0.056
#&gt; GSM38745     1  0.7710      0.161 0.576 0.368 0.056
#&gt; GSM38746     1  0.9485      0.428 0.484 0.212 0.304
#&gt; GSM38747     1  0.9485      0.428 0.484 0.212 0.304
#&gt; GSM38748     3  0.4654      0.609 0.000 0.208 0.792
#&gt; GSM38749     1  0.7966      0.580 0.652 0.220 0.128
#&gt; GSM38750     1  0.9063      0.502 0.552 0.200 0.248
#&gt; GSM38751     1  0.9063      0.502 0.552 0.200 0.248
#&gt; GSM38752     3  0.5404      0.649 0.004 0.256 0.740
#&gt; GSM38753     3  0.6168      0.436 0.000 0.412 0.588
#&gt; GSM38754     3  0.5404      0.649 0.004 0.256 0.740
#&gt; GSM38755     1  0.9553      0.409 0.484 0.244 0.272
#&gt; GSM38756     3  0.6168      0.436 0.000 0.412 0.588
#&gt; GSM38757     1  0.9970      0.117 0.356 0.296 0.348
#&gt; GSM38758     2  0.0475      0.994 0.004 0.992 0.004
#&gt; GSM38759     1  0.4452      0.653 0.808 0.192 0.000
#&gt; GSM38760     1  0.7333      0.606 0.704 0.180 0.116
#&gt; GSM38761     2  0.0475      0.994 0.004 0.992 0.004
#&gt; GSM38762     2  0.0237      0.996 0.004 0.996 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.4967      0.524 0.548 0.000 0.452 0.000
#&gt; GSM38713     1  0.4877      0.579 0.664 0.000 0.328 0.008
#&gt; GSM38714     1  0.4877      0.579 0.664 0.000 0.328 0.008
#&gt; GSM38715     1  0.4877      0.579 0.664 0.000 0.328 0.008
#&gt; GSM38716     1  0.4967      0.524 0.548 0.000 0.452 0.000
#&gt; GSM38717     1  0.3764      0.606 0.784 0.000 0.216 0.000
#&gt; GSM38718     1  0.4624      0.574 0.660 0.000 0.340 0.000
#&gt; GSM38719     1  0.4843      0.554 0.604 0.000 0.396 0.000
#&gt; GSM38720     1  0.4843      0.554 0.604 0.000 0.396 0.000
#&gt; GSM38721     1  0.3283      0.568 0.872 0.020 0.004 0.104
#&gt; GSM38722     1  0.4972      0.521 0.544 0.000 0.456 0.000
#&gt; GSM38723     1  0.4972      0.521 0.544 0.000 0.456 0.000
#&gt; GSM38724     1  0.3910      0.523 0.820 0.024 0.000 0.156
#&gt; GSM38725     1  0.4585      0.576 0.668 0.000 0.332 0.000
#&gt; GSM38726     1  0.4972      0.521 0.544 0.000 0.456 0.000
#&gt; GSM38727     1  0.4972      0.521 0.544 0.000 0.456 0.000
#&gt; GSM38728     1  0.5742      0.396 0.664 0.060 0.000 0.276
#&gt; GSM38729     1  0.4972      0.521 0.544 0.000 0.456 0.000
#&gt; GSM38730     1  0.4972      0.521 0.544 0.000 0.456 0.000
#&gt; GSM38731     1  0.4972      0.521 0.544 0.000 0.456 0.000
#&gt; GSM38732     4  0.5949      0.619 0.288 0.068 0.000 0.644
#&gt; GSM38733     1  0.4689      0.458 0.784 0.044 0.004 0.168
#&gt; GSM38734     4  0.3156      0.765 0.048 0.068 0.000 0.884
#&gt; GSM38735     3  0.0804      1.000 0.008 0.012 0.980 0.000
#&gt; GSM38736     2  0.0469      0.993 0.000 0.988 0.012 0.000
#&gt; GSM38737     2  0.0469      0.993 0.000 0.988 0.012 0.000
#&gt; GSM38738     1  0.5929      0.149 0.640 0.064 0.000 0.296
#&gt; GSM38739     1  0.0592      0.601 0.984 0.000 0.016 0.000
#&gt; GSM38740     3  0.0804      1.000 0.008 0.012 0.980 0.000
#&gt; GSM38741     1  0.4807      0.467 0.728 0.024 0.000 0.248
#&gt; GSM38742     2  0.0000      0.990 0.000 1.000 0.000 0.000
#&gt; GSM38743     2  0.0469      0.993 0.000 0.988 0.012 0.000
#&gt; GSM38744     3  0.0804      1.000 0.008 0.012 0.980 0.000
#&gt; GSM38745     3  0.0804      1.000 0.008 0.012 0.980 0.000
#&gt; GSM38746     1  0.3400      0.552 0.820 0.000 0.000 0.180
#&gt; GSM38747     1  0.3400      0.552 0.820 0.000 0.000 0.180
#&gt; GSM38748     4  0.1042      0.722 0.000 0.020 0.008 0.972
#&gt; GSM38749     1  0.0592      0.601 0.984 0.000 0.016 0.000
#&gt; GSM38750     1  0.2530      0.582 0.888 0.000 0.000 0.112
#&gt; GSM38751     1  0.2530      0.582 0.888 0.000 0.000 0.112
#&gt; GSM38752     4  0.3156      0.765 0.048 0.068 0.000 0.884
#&gt; GSM38753     4  0.5007      0.418 0.000 0.356 0.008 0.636
#&gt; GSM38754     4  0.3156      0.765 0.048 0.068 0.000 0.884
#&gt; GSM38755     1  0.3943      0.483 0.832 0.028 0.004 0.136
#&gt; GSM38756     4  0.5007      0.418 0.000 0.356 0.008 0.636
#&gt; GSM38757     1  0.5767      0.276 0.660 0.060 0.000 0.280
#&gt; GSM38758     2  0.0000      0.990 0.000 1.000 0.000 0.000
#&gt; GSM38759     1  0.4983      0.573 0.704 0.024 0.272 0.000
#&gt; GSM38760     1  0.2704      0.600 0.876 0.000 0.124 0.000
#&gt; GSM38761     2  0.0000      0.990 0.000 1.000 0.000 0.000
#&gt; GSM38762     2  0.0469      0.993 0.000 0.988 0.012 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.5757     0.6012 0.560 0.000 0.000 0.104 0.336
#&gt; GSM38713     1  0.4348     0.6263 0.668 0.000 0.000 0.016 0.316
#&gt; GSM38714     1  0.4348     0.6263 0.668 0.000 0.000 0.016 0.316
#&gt; GSM38715     1  0.4348     0.6263 0.668 0.000 0.000 0.016 0.316
#&gt; GSM38716     1  0.5757     0.6012 0.560 0.000 0.000 0.104 0.336
#&gt; GSM38717     1  0.3427     0.6467 0.796 0.000 0.000 0.012 0.192
#&gt; GSM38718     1  0.4251     0.6247 0.672 0.000 0.000 0.012 0.316
#&gt; GSM38719     1  0.5213     0.6173 0.616 0.000 0.000 0.064 0.320
#&gt; GSM38720     1  0.5213     0.6173 0.616 0.000 0.000 0.064 0.320
#&gt; GSM38721     1  0.3464     0.5577 0.836 0.000 0.096 0.068 0.000
#&gt; GSM38722     1  0.5798     0.5994 0.556 0.000 0.000 0.108 0.336
#&gt; GSM38723     1  0.5798     0.5994 0.556 0.000 0.000 0.108 0.336
#&gt; GSM38724     1  0.3888     0.5205 0.804 0.000 0.120 0.076 0.000
#&gt; GSM38725     1  0.4029     0.6258 0.680 0.000 0.000 0.004 0.316
#&gt; GSM38726     1  0.5798     0.5994 0.556 0.000 0.000 0.108 0.336
#&gt; GSM38727     1  0.5798     0.5994 0.556 0.000 0.000 0.108 0.336
#&gt; GSM38728     1  0.4251     0.3256 0.624 0.000 0.372 0.004 0.000
#&gt; GSM38729     1  0.5798     0.5994 0.556 0.000 0.000 0.108 0.336
#&gt; GSM38730     1  0.5798     0.5994 0.556 0.000 0.000 0.108 0.336
#&gt; GSM38731     1  0.5798     0.5994 0.556 0.000 0.000 0.108 0.336
#&gt; GSM38732     3  0.4707     0.5654 0.228 0.000 0.708 0.064 0.000
#&gt; GSM38733     1  0.4522     0.4275 0.736 0.000 0.196 0.068 0.000
#&gt; GSM38734     3  0.0404     0.8316 0.000 0.000 0.988 0.012 0.000
#&gt; GSM38735     5  0.0912     1.0000 0.000 0.016 0.000 0.012 0.972
#&gt; GSM38736     2  0.0162     0.9858 0.000 0.996 0.000 0.000 0.004
#&gt; GSM38737     2  0.0162     0.9858 0.000 0.996 0.000 0.000 0.004
#&gt; GSM38738     1  0.5357     0.0775 0.588 0.000 0.344 0.068 0.000
#&gt; GSM38739     1  0.0609     0.6269 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38740     5  0.0912     1.0000 0.000 0.016 0.000 0.012 0.972
#&gt; GSM38741     1  0.3885     0.4388 0.724 0.000 0.268 0.008 0.000
#&gt; GSM38742     2  0.0510     0.9809 0.000 0.984 0.000 0.016 0.000
#&gt; GSM38743     2  0.0162     0.9858 0.000 0.996 0.000 0.000 0.004
#&gt; GSM38744     5  0.0912     1.0000 0.000 0.016 0.000 0.012 0.972
#&gt; GSM38745     5  0.0912     1.0000 0.000 0.016 0.000 0.012 0.972
#&gt; GSM38746     1  0.3898     0.5532 0.804 0.000 0.116 0.080 0.000
#&gt; GSM38747     1  0.3898     0.5532 0.804 0.000 0.116 0.080 0.000
#&gt; GSM38748     4  0.3913     0.4368 0.000 0.000 0.324 0.676 0.000
#&gt; GSM38749     1  0.0609     0.6269 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38750     1  0.2914     0.5834 0.872 0.000 0.052 0.076 0.000
#&gt; GSM38751     1  0.2914     0.5834 0.872 0.000 0.052 0.076 0.000
#&gt; GSM38752     3  0.0000     0.8392 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38753     4  0.3242     0.7782 0.000 0.216 0.000 0.784 0.000
#&gt; GSM38754     3  0.0000     0.8392 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38755     1  0.3622     0.4768 0.820 0.000 0.124 0.056 0.000
#&gt; GSM38756     4  0.3242     0.7782 0.000 0.216 0.000 0.784 0.000
#&gt; GSM38757     1  0.5289     0.2377 0.616 0.000 0.312 0.072 0.000
#&gt; GSM38758     2  0.0510     0.9809 0.000 0.984 0.000 0.016 0.000
#&gt; GSM38759     1  0.4595     0.6115 0.716 0.004 0.000 0.044 0.236
#&gt; GSM38760     1  0.2676     0.6346 0.884 0.000 0.000 0.080 0.036
#&gt; GSM38761     2  0.0510     0.9809 0.000 0.984 0.000 0.016 0.000
#&gt; GSM38762     2  0.0162     0.9858 0.000 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.0146      0.902 0.996 0.00 0.004 0.000 0.000 0.000
#&gt; GSM38713     1  0.3361      0.856 0.816 0.00 0.076 0.108 0.000 0.000
#&gt; GSM38714     1  0.3361      0.856 0.816 0.00 0.076 0.108 0.000 0.000
#&gt; GSM38715     1  0.3361      0.856 0.816 0.00 0.076 0.108 0.000 0.000
#&gt; GSM38716     1  0.0291      0.902 0.992 0.00 0.004 0.004 0.000 0.000
#&gt; GSM38717     1  0.4947      0.579 0.636 0.00 0.244 0.120 0.000 0.000
#&gt; GSM38718     1  0.3211      0.861 0.824 0.00 0.056 0.120 0.000 0.000
#&gt; GSM38719     1  0.2066      0.890 0.908 0.00 0.040 0.052 0.000 0.000
#&gt; GSM38720     1  0.2066      0.890 0.908 0.00 0.040 0.052 0.000 0.000
#&gt; GSM38721     3  0.3494      0.746 0.020 0.00 0.828 0.072 0.000 0.080
#&gt; GSM38722     1  0.0458      0.899 0.984 0.00 0.000 0.016 0.000 0.000
#&gt; GSM38723     1  0.0458      0.899 0.984 0.00 0.000 0.016 0.000 0.000
#&gt; GSM38724     3  0.1285      0.759 0.000 0.00 0.944 0.004 0.000 0.052
#&gt; GSM38725     1  0.3196      0.862 0.828 0.00 0.064 0.108 0.000 0.000
#&gt; GSM38726     1  0.0260      0.901 0.992 0.00 0.000 0.008 0.000 0.000
#&gt; GSM38727     1  0.0458      0.899 0.984 0.00 0.000 0.016 0.000 0.000
#&gt; GSM38728     3  0.3634      0.541 0.000 0.00 0.644 0.000 0.000 0.356
#&gt; GSM38729     1  0.0260      0.901 0.992 0.00 0.000 0.008 0.000 0.000
#&gt; GSM38730     1  0.0260      0.901 0.992 0.00 0.000 0.008 0.000 0.000
#&gt; GSM38731     1  0.0363      0.900 0.988 0.00 0.000 0.012 0.000 0.000
#&gt; GSM38732     6  0.4513      0.526 0.012 0.00 0.228 0.060 0.000 0.700
#&gt; GSM38733     3  0.4131      0.688 0.004 0.00 0.744 0.072 0.000 0.180
#&gt; GSM38734     6  0.0291      0.819 0.000 0.00 0.004 0.004 0.000 0.992
#&gt; GSM38735     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000 0.000
#&gt; GSM38736     2  0.0547      0.986 0.000 0.98 0.000 0.000 0.020 0.000
#&gt; GSM38737     2  0.0547      0.986 0.000 0.98 0.000 0.000 0.020 0.000
#&gt; GSM38738     3  0.5016      0.487 0.012 0.00 0.592 0.060 0.000 0.336
#&gt; GSM38739     3  0.2905      0.755 0.084 0.00 0.852 0.064 0.000 0.000
#&gt; GSM38740     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000 0.000
#&gt; GSM38741     3  0.3151      0.633 0.000 0.00 0.748 0.000 0.000 0.252
#&gt; GSM38742     2  0.0000      0.981 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0547      0.986 0.000 0.98 0.000 0.000 0.020 0.000
#&gt; GSM38744     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000 0.000
#&gt; GSM38745     5  0.0000      1.000 0.000 0.00 0.000 0.000 1.000 0.000
#&gt; GSM38746     3  0.1542      0.747 0.008 0.00 0.936 0.052 0.000 0.004
#&gt; GSM38747     3  0.1542      0.747 0.008 0.00 0.936 0.052 0.000 0.004
#&gt; GSM38748     4  0.3699      0.469 0.000 0.00 0.004 0.660 0.000 0.336
#&gt; GSM38749     3  0.2905      0.755 0.084 0.00 0.852 0.064 0.000 0.000
#&gt; GSM38750     3  0.0547      0.768 0.020 0.00 0.980 0.000 0.000 0.000
#&gt; GSM38751     3  0.0547      0.768 0.020 0.00 0.980 0.000 0.000 0.000
#&gt; GSM38752     6  0.0458      0.829 0.000 0.00 0.016 0.000 0.000 0.984
#&gt; GSM38753     4  0.2793      0.798 0.000 0.20 0.000 0.800 0.000 0.000
#&gt; GSM38754     6  0.0458      0.829 0.000 0.00 0.016 0.000 0.000 0.984
#&gt; GSM38755     3  0.4452      0.717 0.056 0.00 0.760 0.060 0.000 0.124
#&gt; GSM38756     4  0.2793      0.798 0.000 0.20 0.000 0.800 0.000 0.000
#&gt; GSM38757     3  0.3518      0.637 0.012 0.00 0.732 0.000 0.000 0.256
#&gt; GSM38758     2  0.0000      0.981 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM38759     3  0.6286      0.483 0.264 0.02 0.572 0.060 0.084 0.000
#&gt; GSM38760     3  0.4446      0.509 0.348 0.00 0.612 0.040 0.000 0.000
#&gt; GSM38761     2  0.0000      0.981 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0547      0.986 0.000 0.98 0.000 0.000 0.020 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n individual(p) k
#> CV:hclust 41      0.157301 2
#> CV:hclust 34      0.002249 3
#> CV:hclust 43      0.000987 4
#> CV:hclust 44      0.000811 5
#> CV:hclust 48      0.005963 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.437           0.794       0.886         0.4680 0.500   0.500
#> 3 3 0.662           0.781       0.883         0.3493 0.695   0.463
#> 4 4 0.579           0.611       0.770         0.1149 0.976   0.929
#> 5 5 0.644           0.667       0.764         0.0807 0.933   0.788
#> 6 6 0.684           0.592       0.727         0.0554 0.875   0.558
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.919 1.000 0.000
#&gt; GSM38713     1   0.000      0.919 1.000 0.000
#&gt; GSM38714     1   0.000      0.919 1.000 0.000
#&gt; GSM38715     1   0.000      0.919 1.000 0.000
#&gt; GSM38716     1   0.000      0.919 1.000 0.000
#&gt; GSM38717     1   0.000      0.919 1.000 0.000
#&gt; GSM38718     1   0.000      0.919 1.000 0.000
#&gt; GSM38719     1   0.000      0.919 1.000 0.000
#&gt; GSM38720     1   0.000      0.919 1.000 0.000
#&gt; GSM38721     1   0.494      0.797 0.892 0.108
#&gt; GSM38722     1   0.000      0.919 1.000 0.000
#&gt; GSM38723     1   0.000      0.919 1.000 0.000
#&gt; GSM38724     2   0.971      0.612 0.400 0.600
#&gt; GSM38725     1   0.000      0.919 1.000 0.000
#&gt; GSM38726     1   0.000      0.919 1.000 0.000
#&gt; GSM38727     1   0.000      0.919 1.000 0.000
#&gt; GSM38728     2   0.998      0.438 0.472 0.528
#&gt; GSM38729     1   0.000      0.919 1.000 0.000
#&gt; GSM38730     1   0.000      0.919 1.000 0.000
#&gt; GSM38731     1   0.000      0.919 1.000 0.000
#&gt; GSM38732     2   0.876      0.734 0.296 0.704
#&gt; GSM38733     1   0.000      0.919 1.000 0.000
#&gt; GSM38734     2   0.745      0.782 0.212 0.788
#&gt; GSM38735     1   0.913      0.551 0.672 0.328
#&gt; GSM38736     2   0.000      0.777 0.000 1.000
#&gt; GSM38737     2   0.000      0.777 0.000 1.000
#&gt; GSM38738     2   0.929      0.694 0.344 0.656
#&gt; GSM38739     1   0.000      0.919 1.000 0.000
#&gt; GSM38740     1   0.753      0.693 0.784 0.216
#&gt; GSM38741     2   0.745      0.782 0.212 0.788
#&gt; GSM38742     2   0.000      0.777 0.000 1.000
#&gt; GSM38743     2   0.000      0.777 0.000 1.000
#&gt; GSM38744     1   0.753      0.693 0.784 0.216
#&gt; GSM38745     1   0.839      0.630 0.732 0.268
#&gt; GSM38746     1   0.821      0.532 0.744 0.256
#&gt; GSM38747     1   0.821      0.532 0.744 0.256
#&gt; GSM38748     2   0.574      0.790 0.136 0.864
#&gt; GSM38749     1   0.000      0.919 1.000 0.000
#&gt; GSM38750     2   0.932      0.690 0.348 0.652
#&gt; GSM38751     2   0.932      0.690 0.348 0.652
#&gt; GSM38752     2   0.745      0.782 0.212 0.788
#&gt; GSM38753     2   0.204      0.785 0.032 0.968
#&gt; GSM38754     2   0.745      0.782 0.212 0.788
#&gt; GSM38755     2   0.999      0.446 0.480 0.520
#&gt; GSM38756     2   0.204      0.785 0.032 0.968
#&gt; GSM38757     2   0.932      0.690 0.348 0.652
#&gt; GSM38758     2   0.000      0.777 0.000 1.000
#&gt; GSM38759     1   0.373      0.848 0.928 0.072
#&gt; GSM38760     1   0.000      0.919 1.000 0.000
#&gt; GSM38761     2   0.000      0.777 0.000 1.000
#&gt; GSM38762     2   0.000      0.777 0.000 1.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM38713     1  0.2625      0.901 0.916 0.000 0.084
#&gt; GSM38714     1  0.2625      0.901 0.916 0.000 0.084
#&gt; GSM38715     1  0.2625      0.901 0.916 0.000 0.084
#&gt; GSM38716     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM38721     3  0.6330      0.487 0.396 0.004 0.600
#&gt; GSM38722     1  0.1170      0.961 0.976 0.016 0.008
#&gt; GSM38723     1  0.1170      0.961 0.976 0.016 0.008
#&gt; GSM38724     3  0.1765      0.834 0.040 0.004 0.956
#&gt; GSM38725     1  0.0237      0.966 0.996 0.004 0.000
#&gt; GSM38726     1  0.0592      0.964 0.988 0.012 0.000
#&gt; GSM38727     1  0.1170      0.961 0.976 0.016 0.008
#&gt; GSM38728     3  0.4164      0.776 0.144 0.008 0.848
#&gt; GSM38729     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM38730     1  0.0237      0.966 0.996 0.004 0.000
#&gt; GSM38731     1  0.0592      0.964 0.988 0.012 0.000
#&gt; GSM38732     3  0.1711      0.836 0.032 0.008 0.960
#&gt; GSM38733     3  0.6168      0.452 0.412 0.000 0.588
#&gt; GSM38734     3  0.0892      0.816 0.000 0.020 0.980
#&gt; GSM38735     2  0.6973      0.374 0.416 0.564 0.020
#&gt; GSM38736     2  0.2625      0.732 0.000 0.916 0.084
#&gt; GSM38737     2  0.2625      0.732 0.000 0.916 0.084
#&gt; GSM38738     3  0.1585      0.836 0.028 0.008 0.964
#&gt; GSM38739     1  0.2176      0.944 0.948 0.020 0.032
#&gt; GSM38740     2  0.7004      0.348 0.428 0.552 0.020
#&gt; GSM38741     3  0.1015      0.826 0.012 0.008 0.980
#&gt; GSM38742     2  0.2625      0.732 0.000 0.916 0.084
#&gt; GSM38743     2  0.2625      0.732 0.000 0.916 0.084
#&gt; GSM38744     2  0.7004      0.348 0.428 0.552 0.020
#&gt; GSM38745     2  0.6973      0.374 0.416 0.564 0.020
#&gt; GSM38746     3  0.6205      0.584 0.336 0.008 0.656
#&gt; GSM38747     3  0.6357      0.587 0.336 0.012 0.652
#&gt; GSM38748     3  0.1753      0.809 0.000 0.048 0.952
#&gt; GSM38749     1  0.2176      0.944 0.948 0.020 0.032
#&gt; GSM38750     3  0.1399      0.837 0.028 0.004 0.968
#&gt; GSM38751     3  0.1399      0.837 0.028 0.004 0.968
#&gt; GSM38752     3  0.0892      0.816 0.000 0.020 0.980
#&gt; GSM38753     2  0.6225      0.293 0.000 0.568 0.432
#&gt; GSM38754     3  0.0892      0.816 0.000 0.020 0.980
#&gt; GSM38755     3  0.4033      0.761 0.136 0.008 0.856
#&gt; GSM38756     2  0.6235      0.283 0.000 0.564 0.436
#&gt; GSM38757     3  0.1399      0.837 0.028 0.004 0.968
#&gt; GSM38758     2  0.2625      0.732 0.000 0.916 0.084
#&gt; GSM38759     1  0.2772      0.903 0.916 0.004 0.080
#&gt; GSM38760     1  0.1170      0.961 0.976 0.016 0.008
#&gt; GSM38761     2  0.2625      0.732 0.000 0.916 0.084
#&gt; GSM38762     2  0.2625      0.732 0.000 0.916 0.084
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.1389     0.8584 0.952 0.000 0.000 0.048
#&gt; GSM38713     1  0.5842     0.7264 0.704 0.000 0.128 0.168
#&gt; GSM38714     1  0.5842     0.7264 0.704 0.000 0.128 0.168
#&gt; GSM38715     1  0.5582     0.7416 0.724 0.000 0.108 0.168
#&gt; GSM38716     1  0.0817     0.8584 0.976 0.000 0.000 0.024
#&gt; GSM38717     1  0.2831     0.8352 0.876 0.000 0.004 0.120
#&gt; GSM38718     1  0.4123     0.8099 0.820 0.000 0.044 0.136
#&gt; GSM38719     1  0.1637     0.8561 0.940 0.000 0.000 0.060
#&gt; GSM38720     1  0.1211     0.8589 0.960 0.000 0.000 0.040
#&gt; GSM38721     3  0.7079     0.4072 0.276 0.000 0.556 0.168
#&gt; GSM38722     1  0.0592     0.8546 0.984 0.000 0.000 0.016
#&gt; GSM38723     1  0.0592     0.8546 0.984 0.000 0.000 0.016
#&gt; GSM38724     3  0.2714     0.6825 0.004 0.000 0.884 0.112
#&gt; GSM38725     1  0.1610     0.8598 0.952 0.000 0.016 0.032
#&gt; GSM38726     1  0.0188     0.8583 0.996 0.000 0.000 0.004
#&gt; GSM38727     1  0.0592     0.8546 0.984 0.000 0.000 0.016
#&gt; GSM38728     3  0.4565     0.6656 0.064 0.000 0.796 0.140
#&gt; GSM38729     1  0.0707     0.8582 0.980 0.000 0.000 0.020
#&gt; GSM38730     1  0.0188     0.8582 0.996 0.000 0.000 0.004
#&gt; GSM38731     1  0.0469     0.8559 0.988 0.000 0.000 0.012
#&gt; GSM38732     3  0.1474     0.6997 0.000 0.000 0.948 0.052
#&gt; GSM38733     3  0.6946     0.4440 0.252 0.000 0.580 0.168
#&gt; GSM38734     3  0.4720     0.4402 0.000 0.004 0.672 0.324
#&gt; GSM38735     2  0.7843     0.0192 0.264 0.372 0.000 0.364
#&gt; GSM38736     2  0.0188     0.6445 0.000 0.996 0.004 0.000
#&gt; GSM38737     2  0.0188     0.6445 0.000 0.996 0.004 0.000
#&gt; GSM38738     3  0.1302     0.7002 0.000 0.000 0.956 0.044
#&gt; GSM38739     1  0.5339     0.5777 0.688 0.000 0.272 0.040
#&gt; GSM38740     4  0.7844    -0.4085 0.264 0.368 0.000 0.368
#&gt; GSM38741     3  0.2408     0.6570 0.000 0.000 0.896 0.104
#&gt; GSM38742     2  0.1109     0.6295 0.000 0.968 0.004 0.028
#&gt; GSM38743     2  0.0188     0.6445 0.000 0.996 0.004 0.000
#&gt; GSM38744     2  0.7844     0.0142 0.264 0.368 0.000 0.368
#&gt; GSM38745     2  0.7844     0.0142 0.264 0.368 0.000 0.368
#&gt; GSM38746     3  0.5728     0.5614 0.188 0.000 0.708 0.104
#&gt; GSM38747     3  0.5728     0.5614 0.188 0.000 0.708 0.104
#&gt; GSM38748     3  0.5161     0.1472 0.000 0.004 0.520 0.476
#&gt; GSM38749     1  0.5339     0.5777 0.688 0.000 0.272 0.040
#&gt; GSM38750     3  0.0592     0.6984 0.000 0.000 0.984 0.016
#&gt; GSM38751     3  0.0921     0.7001 0.000 0.000 0.972 0.028
#&gt; GSM38752     3  0.4655     0.4452 0.000 0.004 0.684 0.312
#&gt; GSM38753     4  0.7899     0.3826 0.000 0.340 0.296 0.364
#&gt; GSM38754     3  0.4655     0.4452 0.000 0.004 0.684 0.312
#&gt; GSM38755     3  0.2319     0.6880 0.036 0.000 0.924 0.040
#&gt; GSM38756     4  0.7782     0.3674 0.000 0.276 0.296 0.428
#&gt; GSM38757     3  0.0000     0.6999 0.000 0.000 1.000 0.000
#&gt; GSM38758     2  0.2944     0.5000 0.000 0.868 0.004 0.128
#&gt; GSM38759     1  0.5800     0.7309 0.708 0.000 0.128 0.164
#&gt; GSM38760     1  0.3763     0.7607 0.832 0.000 0.144 0.024
#&gt; GSM38761     2  0.1109     0.6295 0.000 0.968 0.004 0.028
#&gt; GSM38762     2  0.0188     0.6445 0.000 0.996 0.004 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.2446     0.7318 0.900 0.000 0.000 0.044 0.056
#&gt; GSM38713     1  0.7170     0.5336 0.532 0.000 0.080 0.132 0.256
#&gt; GSM38714     1  0.7170     0.5336 0.532 0.000 0.080 0.132 0.256
#&gt; GSM38715     1  0.7170     0.5336 0.532 0.000 0.080 0.132 0.256
#&gt; GSM38716     1  0.1568     0.7365 0.944 0.000 0.000 0.036 0.020
#&gt; GSM38717     1  0.4199     0.6892 0.772 0.000 0.000 0.068 0.160
#&gt; GSM38718     1  0.5633     0.6302 0.664 0.000 0.024 0.084 0.228
#&gt; GSM38719     1  0.3090     0.7196 0.860 0.000 0.000 0.052 0.088
#&gt; GSM38720     1  0.2376     0.7323 0.904 0.000 0.000 0.044 0.052
#&gt; GSM38721     3  0.7946     0.3751 0.184 0.000 0.452 0.136 0.228
#&gt; GSM38722     1  0.0693     0.7307 0.980 0.000 0.000 0.008 0.012
#&gt; GSM38723     1  0.0912     0.7284 0.972 0.000 0.000 0.012 0.016
#&gt; GSM38724     3  0.3898     0.6673 0.000 0.000 0.804 0.080 0.116
#&gt; GSM38725     1  0.1280     0.7415 0.960 0.000 0.008 0.008 0.024
#&gt; GSM38726     1  0.0693     0.7312 0.980 0.000 0.000 0.008 0.012
#&gt; GSM38727     1  0.0912     0.7284 0.972 0.000 0.000 0.012 0.016
#&gt; GSM38728     3  0.5716     0.5322 0.008 0.000 0.652 0.168 0.172
#&gt; GSM38729     1  0.0693     0.7362 0.980 0.000 0.000 0.008 0.012
#&gt; GSM38730     1  0.0000     0.7350 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0566     0.7309 0.984 0.000 0.000 0.004 0.012
#&gt; GSM38732     3  0.3608     0.6216 0.000 0.000 0.824 0.064 0.112
#&gt; GSM38733     3  0.7518     0.4505 0.124 0.000 0.512 0.136 0.228
#&gt; GSM38734     4  0.5276     0.5343 0.000 0.000 0.436 0.516 0.048
#&gt; GSM38735     5  0.5854     0.9952 0.160 0.240 0.000 0.000 0.600
#&gt; GSM38736     2  0.0451     0.9537 0.000 0.988 0.004 0.000 0.008
#&gt; GSM38737     2  0.0451     0.9537 0.000 0.988 0.004 0.000 0.008
#&gt; GSM38738     3  0.1753     0.6718 0.000 0.000 0.936 0.032 0.032
#&gt; GSM38739     1  0.6577    -0.0036 0.456 0.000 0.408 0.024 0.112
#&gt; GSM38740     5  0.5864     0.9952 0.164 0.236 0.000 0.000 0.600
#&gt; GSM38741     3  0.3596     0.4165 0.000 0.000 0.784 0.200 0.016
#&gt; GSM38742     2  0.0771     0.9459 0.000 0.976 0.004 0.020 0.000
#&gt; GSM38743     2  0.0451     0.9537 0.000 0.988 0.004 0.000 0.008
#&gt; GSM38744     5  0.5864     0.9952 0.164 0.236 0.000 0.000 0.600
#&gt; GSM38745     5  0.5854     0.9952 0.160 0.240 0.000 0.000 0.600
#&gt; GSM38746     3  0.5013     0.6392 0.052 0.000 0.756 0.068 0.124
#&gt; GSM38747     3  0.5432     0.6329 0.052 0.000 0.712 0.064 0.172
#&gt; GSM38748     4  0.2806     0.6155 0.000 0.000 0.152 0.844 0.004
#&gt; GSM38749     1  0.6577    -0.0036 0.456 0.000 0.408 0.024 0.112
#&gt; GSM38750     3  0.2248     0.6490 0.000 0.000 0.900 0.012 0.088
#&gt; GSM38751     3  0.2351     0.6489 0.000 0.000 0.896 0.016 0.088
#&gt; GSM38752     4  0.5216     0.5443 0.000 0.000 0.436 0.520 0.044
#&gt; GSM38753     4  0.5264     0.4049 0.000 0.264 0.068 0.660 0.008
#&gt; GSM38754     4  0.5216     0.5443 0.000 0.000 0.436 0.520 0.044
#&gt; GSM38755     3  0.1996     0.6809 0.004 0.000 0.928 0.032 0.036
#&gt; GSM38756     4  0.4696     0.5068 0.000 0.184 0.068 0.740 0.008
#&gt; GSM38757     3  0.0000     0.6698 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38758     2  0.2536     0.8215 0.000 0.868 0.004 0.128 0.000
#&gt; GSM38759     1  0.7003     0.5441 0.548 0.000 0.068 0.132 0.252
#&gt; GSM38760     1  0.5375     0.4829 0.684 0.000 0.216 0.016 0.084
#&gt; GSM38761     2  0.0865     0.9452 0.000 0.972 0.004 0.024 0.000
#&gt; GSM38762     2  0.0613     0.9530 0.000 0.984 0.004 0.004 0.008
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.3212      0.728 0.800 0.000 0.000 0.004 0.016 0.180
#&gt; GSM38713     6  0.4649      0.556 0.324 0.000 0.016 0.004 0.024 0.632
#&gt; GSM38714     6  0.4788      0.551 0.324 0.000 0.016 0.004 0.032 0.624
#&gt; GSM38715     6  0.4649      0.556 0.324 0.000 0.016 0.004 0.024 0.632
#&gt; GSM38716     1  0.2489      0.764 0.860 0.000 0.000 0.000 0.012 0.128
#&gt; GSM38717     1  0.4182      0.507 0.660 0.000 0.000 0.004 0.024 0.312
#&gt; GSM38718     1  0.4746      0.174 0.544 0.000 0.004 0.004 0.032 0.416
#&gt; GSM38719     1  0.3430      0.697 0.772 0.000 0.000 0.004 0.016 0.208
#&gt; GSM38720     1  0.3178      0.730 0.804 0.000 0.000 0.004 0.016 0.176
#&gt; GSM38721     6  0.4508      0.457 0.096 0.000 0.120 0.008 0.020 0.756
#&gt; GSM38722     1  0.0622      0.773 0.980 0.000 0.008 0.000 0.012 0.000
#&gt; GSM38723     1  0.1261      0.760 0.956 0.000 0.008 0.004 0.028 0.004
#&gt; GSM38724     3  0.4533      0.373 0.000 0.000 0.504 0.004 0.024 0.468
#&gt; GSM38725     1  0.2780      0.765 0.868 0.000 0.004 0.008 0.024 0.096
#&gt; GSM38726     1  0.0146      0.777 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM38727     1  0.0951      0.766 0.968 0.000 0.008 0.000 0.020 0.004
#&gt; GSM38728     6  0.5931     -0.279 0.000 0.000 0.240 0.148 0.036 0.576
#&gt; GSM38729     1  0.1913      0.782 0.908 0.000 0.000 0.000 0.012 0.080
#&gt; GSM38730     1  0.1411      0.786 0.936 0.000 0.000 0.000 0.004 0.060
#&gt; GSM38731     1  0.0146      0.779 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM38732     3  0.5936      0.228 0.000 0.000 0.460 0.048 0.076 0.416
#&gt; GSM38733     6  0.4323      0.372 0.060 0.000 0.132 0.008 0.028 0.772
#&gt; GSM38734     4  0.7158      0.273 0.000 0.000 0.272 0.360 0.080 0.288
#&gt; GSM38735     5  0.4018      0.984 0.040 0.168 0.000 0.008 0.772 0.012
#&gt; GSM38736     2  0.0632      0.939 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; GSM38737     2  0.0632      0.939 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; GSM38738     3  0.4749      0.484 0.000 0.000 0.676 0.016 0.064 0.244
#&gt; GSM38739     3  0.5226      0.257 0.376 0.000 0.556 0.008 0.044 0.016
#&gt; GSM38740     5  0.3481      0.984 0.048 0.160 0.000 0.000 0.792 0.000
#&gt; GSM38741     3  0.6349      0.141 0.000 0.000 0.552 0.196 0.064 0.188
#&gt; GSM38742     2  0.0632      0.930 0.000 0.976 0.000 0.024 0.000 0.000
#&gt; GSM38743     2  0.0632      0.939 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; GSM38744     5  0.3481      0.984 0.048 0.160 0.000 0.000 0.792 0.000
#&gt; GSM38745     5  0.4018      0.984 0.040 0.168 0.000 0.008 0.772 0.012
#&gt; GSM38746     3  0.3419      0.520 0.012 0.000 0.792 0.000 0.016 0.180
#&gt; GSM38747     3  0.3748      0.494 0.012 0.000 0.756 0.000 0.020 0.212
#&gt; GSM38748     4  0.1630      0.472 0.000 0.000 0.024 0.940 0.016 0.020
#&gt; GSM38749     3  0.5226      0.257 0.376 0.000 0.556 0.008 0.044 0.016
#&gt; GSM38750     3  0.0458      0.541 0.000 0.000 0.984 0.000 0.000 0.016
#&gt; GSM38751     3  0.0603      0.541 0.000 0.000 0.980 0.000 0.004 0.016
#&gt; GSM38752     4  0.6967      0.354 0.000 0.000 0.304 0.400 0.068 0.228
#&gt; GSM38753     4  0.3693      0.336 0.000 0.216 0.012 0.756 0.016 0.000
#&gt; GSM38754     4  0.6967      0.354 0.000 0.000 0.304 0.400 0.068 0.228
#&gt; GSM38755     3  0.5079      0.485 0.000 0.000 0.640 0.016 0.084 0.260
#&gt; GSM38756     4  0.3352      0.389 0.000 0.172 0.012 0.800 0.016 0.000
#&gt; GSM38757     3  0.3714      0.514 0.000 0.000 0.760 0.000 0.044 0.196
#&gt; GSM38758     2  0.3011      0.835 0.000 0.852 0.000 0.100 0.012 0.036
#&gt; GSM38759     6  0.4843      0.497 0.340 0.000 0.004 0.016 0.032 0.608
#&gt; GSM38760     1  0.4648      0.305 0.636 0.000 0.312 0.004 0.044 0.004
#&gt; GSM38761     2  0.1575      0.918 0.000 0.936 0.000 0.032 0.000 0.032
#&gt; GSM38762     2  0.1405      0.935 0.000 0.948 0.000 0.004 0.024 0.024
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n individual(p) k
#> CV:kmeans 49      0.062029 2
#> CV:kmeans 43      0.002090 3
#> CV:kmeans 39      0.008902 4
#> CV:kmeans 44      0.000842 5
#> CV:kmeans 31      0.000702 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:skmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.841           0.929       0.970         0.5077 0.492   0.492
#> 3 3 0.880           0.932       0.970         0.3085 0.737   0.517
#> 4 4 0.718           0.610       0.823         0.1139 0.976   0.929
#> 5 5 0.682           0.487       0.707         0.0582 0.786   0.412
#> 6 6 0.756           0.699       0.835         0.0483 0.876   0.511
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.967 1.000 0.000
#&gt; GSM38713     1   0.000      0.967 1.000 0.000
#&gt; GSM38714     1   0.000      0.967 1.000 0.000
#&gt; GSM38715     1   0.000      0.967 1.000 0.000
#&gt; GSM38716     1   0.000      0.967 1.000 0.000
#&gt; GSM38717     1   0.000      0.967 1.000 0.000
#&gt; GSM38718     1   0.000      0.967 1.000 0.000
#&gt; GSM38719     1   0.000      0.967 1.000 0.000
#&gt; GSM38720     1   0.000      0.967 1.000 0.000
#&gt; GSM38721     1   0.855      0.594 0.720 0.280
#&gt; GSM38722     1   0.000      0.967 1.000 0.000
#&gt; GSM38723     1   0.000      0.967 1.000 0.000
#&gt; GSM38724     2   0.000      0.967 0.000 1.000
#&gt; GSM38725     1   0.000      0.967 1.000 0.000
#&gt; GSM38726     1   0.000      0.967 1.000 0.000
#&gt; GSM38727     1   0.000      0.967 1.000 0.000
#&gt; GSM38728     2   0.000      0.967 0.000 1.000
#&gt; GSM38729     1   0.000      0.967 1.000 0.000
#&gt; GSM38730     1   0.000      0.967 1.000 0.000
#&gt; GSM38731     1   0.000      0.967 1.000 0.000
#&gt; GSM38732     2   0.000      0.967 0.000 1.000
#&gt; GSM38733     1   0.000      0.967 1.000 0.000
#&gt; GSM38734     2   0.000      0.967 0.000 1.000
#&gt; GSM38735     1   0.722      0.755 0.800 0.200
#&gt; GSM38736     2   0.000      0.967 0.000 1.000
#&gt; GSM38737     2   0.000      0.967 0.000 1.000
#&gt; GSM38738     2   0.000      0.967 0.000 1.000
#&gt; GSM38739     1   0.000      0.967 1.000 0.000
#&gt; GSM38740     1   0.000      0.967 1.000 0.000
#&gt; GSM38741     2   0.000      0.967 0.000 1.000
#&gt; GSM38742     2   0.000      0.967 0.000 1.000
#&gt; GSM38743     2   0.000      0.967 0.000 1.000
#&gt; GSM38744     1   0.000      0.967 1.000 0.000
#&gt; GSM38745     1   0.529      0.854 0.880 0.120
#&gt; GSM38746     2   0.518      0.849 0.116 0.884
#&gt; GSM38747     2   0.971      0.295 0.400 0.600
#&gt; GSM38748     2   0.000      0.967 0.000 1.000
#&gt; GSM38749     1   0.000      0.967 1.000 0.000
#&gt; GSM38750     2   0.000      0.967 0.000 1.000
#&gt; GSM38751     2   0.000      0.967 0.000 1.000
#&gt; GSM38752     2   0.000      0.967 0.000 1.000
#&gt; GSM38753     2   0.000      0.967 0.000 1.000
#&gt; GSM38754     2   0.000      0.967 0.000 1.000
#&gt; GSM38755     2   0.722      0.738 0.200 0.800
#&gt; GSM38756     2   0.000      0.967 0.000 1.000
#&gt; GSM38757     2   0.000      0.967 0.000 1.000
#&gt; GSM38758     2   0.000      0.967 0.000 1.000
#&gt; GSM38759     1   0.722      0.755 0.800 0.200
#&gt; GSM38760     1   0.000      0.967 1.000 0.000
#&gt; GSM38761     2   0.000      0.967 0.000 1.000
#&gt; GSM38762     2   0.000      0.967 0.000 1.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38713     1  0.0892      0.969 0.980 0.000 0.020
#&gt; GSM38714     1  0.0892      0.969 0.980 0.000 0.020
#&gt; GSM38715     1  0.0892      0.969 0.980 0.000 0.020
#&gt; GSM38716     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38721     3  0.4399      0.786 0.188 0.000 0.812
#&gt; GSM38722     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38724     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38725     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38728     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38729     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38732     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38733     3  0.4178      0.803 0.172 0.000 0.828
#&gt; GSM38734     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38735     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38736     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38738     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38739     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38740     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38741     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38742     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38744     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38745     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38746     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38747     3  0.4452      0.781 0.192 0.000 0.808
#&gt; GSM38748     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38749     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38750     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38751     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38752     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38753     2  0.4452      0.769 0.000 0.808 0.192
#&gt; GSM38754     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38755     3  0.3192      0.854 0.112 0.000 0.888
#&gt; GSM38756     2  0.6026      0.446 0.000 0.624 0.376
#&gt; GSM38757     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM38758     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38759     1  0.5216      0.643 0.740 0.260 0.000
#&gt; GSM38760     1  0.0000      0.983 1.000 0.000 0.000
#&gt; GSM38761     2  0.0000      0.952 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000      0.952 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.2530     0.7745 0.888 0.000 0.000 0.112
#&gt; GSM38713     1  0.5039     0.6233 0.592 0.000 0.004 0.404
#&gt; GSM38714     1  0.5039     0.6233 0.592 0.000 0.004 0.404
#&gt; GSM38715     1  0.5039     0.6233 0.592 0.000 0.004 0.404
#&gt; GSM38716     1  0.1474     0.7825 0.948 0.000 0.000 0.052
#&gt; GSM38717     1  0.3356     0.7551 0.824 0.000 0.000 0.176
#&gt; GSM38718     1  0.4713     0.6577 0.640 0.000 0.000 0.360
#&gt; GSM38719     1  0.3219     0.7597 0.836 0.000 0.000 0.164
#&gt; GSM38720     1  0.2589     0.7737 0.884 0.000 0.000 0.116
#&gt; GSM38721     3  0.7589     0.0632 0.196 0.000 0.404 0.400
#&gt; GSM38722     1  0.0188     0.7831 0.996 0.000 0.000 0.004
#&gt; GSM38723     1  0.0336     0.7816 0.992 0.000 0.000 0.008
#&gt; GSM38724     3  0.0469     0.6629 0.000 0.000 0.988 0.012
#&gt; GSM38725     1  0.0000     0.7842 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000     0.7842 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0336     0.7816 0.992 0.000 0.000 0.008
#&gt; GSM38728     3  0.1637     0.6116 0.000 0.000 0.940 0.060
#&gt; GSM38729     1  0.0000     0.7842 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000     0.7842 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000     0.7842 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.0336     0.6638 0.000 0.000 0.992 0.008
#&gt; GSM38733     3  0.5766     0.2200 0.032 0.000 0.564 0.404
#&gt; GSM38734     3  0.0000     0.6657 0.000 0.000 1.000 0.000
#&gt; GSM38735     2  0.2469     0.8675 0.000 0.892 0.000 0.108
#&gt; GSM38736     2  0.0000     0.8923 0.000 1.000 0.000 0.000
#&gt; GSM38737     2  0.0000     0.8923 0.000 1.000 0.000 0.000
#&gt; GSM38738     3  0.0592     0.6556 0.000 0.000 0.984 0.016
#&gt; GSM38739     1  0.4998     0.1181 0.512 0.000 0.000 0.488
#&gt; GSM38740     2  0.2469     0.8675 0.000 0.892 0.000 0.108
#&gt; GSM38741     3  0.0336     0.6632 0.000 0.000 0.992 0.008
#&gt; GSM38742     2  0.0000     0.8923 0.000 1.000 0.000 0.000
#&gt; GSM38743     2  0.0000     0.8923 0.000 1.000 0.000 0.000
#&gt; GSM38744     2  0.2469     0.8675 0.000 0.892 0.000 0.108
#&gt; GSM38745     2  0.2469     0.8675 0.000 0.892 0.000 0.108
#&gt; GSM38746     4  0.4998     0.9022 0.000 0.000 0.488 0.512
#&gt; GSM38747     4  0.5132     0.9055 0.004 0.000 0.448 0.548
#&gt; GSM38748     3  0.0000     0.6657 0.000 0.000 1.000 0.000
#&gt; GSM38749     1  0.4998     0.1181 0.512 0.000 0.000 0.488
#&gt; GSM38750     3  0.4994    -0.8886 0.000 0.000 0.520 0.480
#&gt; GSM38751     3  0.4998    -0.9003 0.000 0.000 0.512 0.488
#&gt; GSM38752     3  0.0188     0.6653 0.000 0.000 0.996 0.004
#&gt; GSM38753     2  0.5277     0.5554 0.000 0.668 0.304 0.028
#&gt; GSM38754     3  0.0188     0.6653 0.000 0.000 0.996 0.004
#&gt; GSM38755     3  0.4375     0.3823 0.180 0.000 0.788 0.032
#&gt; GSM38756     2  0.5409     0.1028 0.000 0.496 0.492 0.012
#&gt; GSM38757     3  0.0817     0.6483 0.000 0.000 0.976 0.024
#&gt; GSM38758     2  0.0000     0.8923 0.000 1.000 0.000 0.000
#&gt; GSM38759     1  0.7535     0.4859 0.492 0.156 0.008 0.344
#&gt; GSM38760     1  0.5158     0.1470 0.524 0.000 0.004 0.472
#&gt; GSM38761     2  0.0000     0.8923 0.000 1.000 0.000 0.000
#&gt; GSM38762     2  0.0000     0.8923 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.4101      0.361 0.664 0.000 0.332 0.000 0.004
#&gt; GSM38713     1  0.1410      0.555 0.940 0.060 0.000 0.000 0.000
#&gt; GSM38714     1  0.1410      0.555 0.940 0.060 0.000 0.000 0.000
#&gt; GSM38715     1  0.1410      0.555 0.940 0.060 0.000 0.000 0.000
#&gt; GSM38716     1  0.4649      0.153 0.580 0.000 0.404 0.000 0.016
#&gt; GSM38717     1  0.3534      0.458 0.744 0.000 0.256 0.000 0.000
#&gt; GSM38718     1  0.1608      0.542 0.928 0.000 0.072 0.000 0.000
#&gt; GSM38719     1  0.3661      0.443 0.724 0.000 0.276 0.000 0.000
#&gt; GSM38720     1  0.4084      0.368 0.668 0.000 0.328 0.000 0.004
#&gt; GSM38721     1  0.5342      0.186 0.612 0.076 0.000 0.312 0.000
#&gt; GSM38722     3  0.4882      0.143 0.444 0.000 0.532 0.000 0.024
#&gt; GSM38723     3  0.4867      0.148 0.432 0.000 0.544 0.000 0.024
#&gt; GSM38724     4  0.3333      0.864 0.008 0.164 0.008 0.820 0.000
#&gt; GSM38725     3  0.4807      0.139 0.448 0.000 0.532 0.000 0.020
#&gt; GSM38726     3  0.4807      0.139 0.448 0.000 0.532 0.000 0.020
#&gt; GSM38727     3  0.4948      0.147 0.436 0.000 0.536 0.000 0.028
#&gt; GSM38728     4  0.3051      0.824 0.076 0.060 0.000 0.864 0.000
#&gt; GSM38729     3  0.4894      0.113 0.456 0.000 0.520 0.000 0.024
#&gt; GSM38730     3  0.4807      0.139 0.448 0.000 0.532 0.000 0.020
#&gt; GSM38731     3  0.4882      0.143 0.444 0.000 0.532 0.000 0.024
#&gt; GSM38732     4  0.1168      0.904 0.032 0.008 0.000 0.960 0.000
#&gt; GSM38733     1  0.5513     -0.098 0.524 0.068 0.000 0.408 0.000
#&gt; GSM38734     4  0.0671      0.910 0.004 0.016 0.000 0.980 0.000
#&gt; GSM38735     5  0.0510      0.810 0.000 0.016 0.000 0.000 0.984
#&gt; GSM38736     2  0.3752      0.820 0.000 0.708 0.000 0.000 0.292
#&gt; GSM38737     2  0.3752      0.820 0.000 0.708 0.000 0.000 0.292
#&gt; GSM38738     4  0.0693      0.909 0.012 0.008 0.000 0.980 0.000
#&gt; GSM38739     3  0.1121      0.281 0.000 0.044 0.956 0.000 0.000
#&gt; GSM38740     5  0.0000      0.831 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38741     4  0.1892      0.903 0.000 0.080 0.004 0.916 0.000
#&gt; GSM38742     2  0.3684      0.819 0.000 0.720 0.000 0.000 0.280
#&gt; GSM38743     2  0.3752      0.820 0.000 0.708 0.000 0.000 0.292
#&gt; GSM38744     5  0.0000      0.831 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38745     5  0.0000      0.831 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38746     3  0.6896     -0.200 0.040 0.124 0.472 0.364 0.000
#&gt; GSM38747     3  0.7164     -0.193 0.064 0.120 0.468 0.348 0.000
#&gt; GSM38748     4  0.2020      0.898 0.000 0.100 0.000 0.900 0.000
#&gt; GSM38749     3  0.1197      0.281 0.000 0.048 0.952 0.000 0.000
#&gt; GSM38750     3  0.6100     -0.274 0.000 0.124 0.448 0.428 0.000
#&gt; GSM38751     3  0.6089     -0.244 0.000 0.124 0.468 0.408 0.000
#&gt; GSM38752     4  0.1341      0.912 0.000 0.056 0.000 0.944 0.000
#&gt; GSM38753     2  0.6448      0.309 0.000 0.500 0.000 0.228 0.272
#&gt; GSM38754     4  0.1341      0.912 0.000 0.056 0.000 0.944 0.000
#&gt; GSM38755     4  0.4142      0.773 0.024 0.036 0.128 0.808 0.004
#&gt; GSM38756     2  0.6191      0.322 0.000 0.528 0.000 0.308 0.164
#&gt; GSM38757     4  0.2193      0.874 0.000 0.092 0.008 0.900 0.000
#&gt; GSM38758     2  0.3942      0.805 0.000 0.728 0.000 0.012 0.260
#&gt; GSM38759     5  0.6591      0.390 0.316 0.160 0.004 0.008 0.512
#&gt; GSM38760     3  0.2120      0.268 0.048 0.004 0.924 0.004 0.020
#&gt; GSM38761     2  0.3684      0.819 0.000 0.720 0.000 0.000 0.280
#&gt; GSM38762     2  0.3752      0.820 0.000 0.708 0.000 0.000 0.292
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.2491     0.7573 0.836 0.000 0.000 0.000 0.000 0.164
#&gt; GSM38713     6  0.2730     0.6689 0.192 0.000 0.000 0.000 0.000 0.808
#&gt; GSM38714     6  0.2730     0.6689 0.192 0.000 0.000 0.000 0.000 0.808
#&gt; GSM38715     6  0.2730     0.6689 0.192 0.000 0.000 0.000 0.000 0.808
#&gt; GSM38716     1  0.1863     0.8024 0.896 0.000 0.000 0.000 0.000 0.104
#&gt; GSM38717     1  0.3714     0.4643 0.656 0.000 0.004 0.000 0.000 0.340
#&gt; GSM38718     6  0.3907     0.2703 0.408 0.000 0.004 0.000 0.000 0.588
#&gt; GSM38719     1  0.3337     0.6249 0.736 0.000 0.004 0.000 0.000 0.260
#&gt; GSM38720     1  0.2772     0.7378 0.816 0.000 0.004 0.000 0.000 0.180
#&gt; GSM38721     6  0.4083     0.5009 0.044 0.000 0.012 0.168 0.008 0.768
#&gt; GSM38722     1  0.0291     0.8463 0.992 0.000 0.004 0.000 0.004 0.000
#&gt; GSM38723     1  0.0862     0.8357 0.972 0.000 0.016 0.000 0.008 0.004
#&gt; GSM38724     4  0.4737     0.6022 0.000 0.000 0.248 0.672 0.012 0.068
#&gt; GSM38725     1  0.0436     0.8460 0.988 0.000 0.004 0.000 0.004 0.004
#&gt; GSM38726     1  0.0000     0.8479 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0964     0.8331 0.968 0.000 0.016 0.000 0.012 0.004
#&gt; GSM38728     4  0.4601     0.5601 0.000 0.000 0.044 0.640 0.008 0.308
#&gt; GSM38729     1  0.0508     0.8461 0.984 0.000 0.004 0.000 0.000 0.012
#&gt; GSM38730     1  0.0146     0.8480 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM38731     1  0.0000     0.8479 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     4  0.3277     0.7466 0.000 0.000 0.020 0.828 0.024 0.128
#&gt; GSM38733     6  0.4236     0.3897 0.012 0.000 0.020 0.208 0.020 0.740
#&gt; GSM38734     4  0.2658     0.7529 0.000 0.000 0.008 0.864 0.016 0.112
#&gt; GSM38735     5  0.1285     0.9935 0.004 0.052 0.000 0.000 0.944 0.000
#&gt; GSM38736     2  0.0547     0.8899 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38737     2  0.0547     0.8899 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38738     4  0.3185     0.7503 0.000 0.000 0.024 0.840 0.024 0.112
#&gt; GSM38739     3  0.3767     0.6394 0.276 0.000 0.708 0.000 0.012 0.004
#&gt; GSM38740     5  0.1333     0.9978 0.008 0.048 0.000 0.000 0.944 0.000
#&gt; GSM38741     4  0.2815     0.7141 0.000 0.000 0.096 0.864 0.012 0.028
#&gt; GSM38742     2  0.0000     0.8875 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0547     0.8899 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38744     5  0.1333     0.9978 0.008 0.048 0.000 0.000 0.944 0.000
#&gt; GSM38745     5  0.1333     0.9978 0.008 0.048 0.000 0.000 0.944 0.000
#&gt; GSM38746     3  0.1757     0.7943 0.000 0.000 0.916 0.076 0.000 0.008
#&gt; GSM38747     3  0.2509     0.7763 0.000 0.000 0.876 0.088 0.000 0.036
#&gt; GSM38748     4  0.2655     0.7156 0.000 0.000 0.036 0.884 0.020 0.060
#&gt; GSM38749     3  0.3767     0.6394 0.276 0.000 0.708 0.000 0.012 0.004
#&gt; GSM38750     3  0.2135     0.7630 0.000 0.000 0.872 0.128 0.000 0.000
#&gt; GSM38751     3  0.1714     0.7910 0.000 0.000 0.908 0.092 0.000 0.000
#&gt; GSM38752     4  0.1226     0.7515 0.000 0.004 0.040 0.952 0.004 0.000
#&gt; GSM38753     2  0.7518    -0.0331 0.000 0.380 0.056 0.364 0.132 0.068
#&gt; GSM38754     4  0.1226     0.7515 0.000 0.004 0.040 0.952 0.004 0.000
#&gt; GSM38755     4  0.6262     0.6526 0.056 0.000 0.116 0.640 0.052 0.136
#&gt; GSM38756     4  0.6701    -0.0329 0.000 0.392 0.056 0.444 0.040 0.068
#&gt; GSM38757     4  0.5129     0.6540 0.000 0.000 0.216 0.656 0.016 0.112
#&gt; GSM38758     2  0.0405     0.8800 0.000 0.988 0.000 0.008 0.004 0.000
#&gt; GSM38759     6  0.8609    -0.1095 0.060 0.100 0.052 0.124 0.312 0.352
#&gt; GSM38760     1  0.4318     0.2577 0.632 0.000 0.340 0.000 0.020 0.008
#&gt; GSM38761     2  0.0000     0.8875 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0547     0.8899 0.000 0.980 0.000 0.000 0.020 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n individual(p) k
#> CV:skmeans 50      0.043771 2
#> CV:skmeans 50      0.003367 3
#> CV:skmeans 41      0.003736 4
#> CV:skmeans 26      0.037852 5
#> CV:skmeans 44      0.000724 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.731           0.800       0.924         0.3606 0.633   0.633
#> 3 3 0.888           0.928       0.965         0.7926 0.645   0.473
#> 4 4 0.860           0.920       0.938         0.0707 0.955   0.874
#> 5 5 0.705           0.767       0.876         0.0617 0.835   0.569
#> 6 6 0.864           0.874       0.931         0.1038 0.801   0.409
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38713     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38714     1  0.0672     0.9336 0.992 0.008
#&gt; GSM38715     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38716     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38717     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38718     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38719     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38720     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38721     1  0.0672     0.9336 0.992 0.008
#&gt; GSM38722     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38723     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38724     1  0.0672     0.9336 0.992 0.008
#&gt; GSM38725     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38726     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38727     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38728     1  0.0672     0.9336 0.992 0.008
#&gt; GSM38729     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38730     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38731     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38732     1  0.0672     0.9336 0.992 0.008
#&gt; GSM38733     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38734     1  0.9881     0.0657 0.564 0.436
#&gt; GSM38735     2  0.9732     0.3624 0.404 0.596
#&gt; GSM38736     2  0.0000     0.7951 0.000 1.000
#&gt; GSM38737     2  0.0000     0.7951 0.000 1.000
#&gt; GSM38738     1  0.1184     0.9293 0.984 0.016
#&gt; GSM38739     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38740     1  0.7219     0.6688 0.800 0.200
#&gt; GSM38741     1  0.1633     0.9237 0.976 0.024
#&gt; GSM38742     2  0.0000     0.7951 0.000 1.000
#&gt; GSM38743     2  0.0000     0.7951 0.000 1.000
#&gt; GSM38744     1  0.7219     0.6688 0.800 0.200
#&gt; GSM38745     2  0.9850     0.3077 0.428 0.572
#&gt; GSM38746     1  0.1633     0.9237 0.976 0.024
#&gt; GSM38747     1  0.1633     0.9237 0.976 0.024
#&gt; GSM38748     2  0.9522     0.4566 0.372 0.628
#&gt; GSM38749     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38750     1  0.0672     0.9336 0.992 0.008
#&gt; GSM38751     1  0.1633     0.9237 0.976 0.024
#&gt; GSM38752     1  0.9909     0.0368 0.556 0.444
#&gt; GSM38753     2  0.9522     0.4566 0.372 0.628
#&gt; GSM38754     1  0.9909     0.0368 0.556 0.444
#&gt; GSM38755     1  0.0672     0.9336 0.992 0.008
#&gt; GSM38756     2  0.9522     0.4566 0.372 0.628
#&gt; GSM38757     1  0.0672     0.9336 0.992 0.008
#&gt; GSM38758     2  0.0000     0.7951 0.000 1.000
#&gt; GSM38759     1  0.1633     0.9237 0.976 0.024
#&gt; GSM38760     1  0.0000     0.9356 1.000 0.000
#&gt; GSM38761     2  0.0000     0.7951 0.000 1.000
#&gt; GSM38762     2  0.0000     0.7951 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38713     1  0.4750      0.756 0.784 0.000 0.216
#&gt; GSM38714     3  0.1643      0.938 0.044 0.000 0.956
#&gt; GSM38715     1  0.4555      0.773 0.800 0.000 0.200
#&gt; GSM38716     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38718     1  0.0237      0.941 0.996 0.000 0.004
#&gt; GSM38719     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38721     3  0.1643      0.938 0.044 0.000 0.956
#&gt; GSM38722     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38724     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38725     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38728     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38729     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM38732     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38733     1  0.4887      0.748 0.772 0.000 0.228
#&gt; GSM38734     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38735     2  0.0000      0.994 0.000 1.000 0.000
#&gt; GSM38736     2  0.0000      0.994 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      0.994 0.000 1.000 0.000
#&gt; GSM38738     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38739     1  0.0747      0.934 0.984 0.000 0.016
#&gt; GSM38740     1  0.4605      0.755 0.796 0.204 0.000
#&gt; GSM38741     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38742     2  0.0000      0.994 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000      0.994 0.000 1.000 0.000
#&gt; GSM38744     1  0.4555      0.760 0.800 0.200 0.000
#&gt; GSM38745     2  0.1643      0.951 0.044 0.956 0.000
#&gt; GSM38746     3  0.1163      0.948 0.028 0.000 0.972
#&gt; GSM38747     3  0.1163      0.948 0.028 0.000 0.972
#&gt; GSM38748     3  0.1163      0.944 0.000 0.028 0.972
#&gt; GSM38749     1  0.0747      0.934 0.984 0.000 0.016
#&gt; GSM38750     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38751     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38752     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38753     3  0.4887      0.711 0.000 0.228 0.772
#&gt; GSM38754     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38755     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38756     3  0.4555      0.744 0.000 0.200 0.800
#&gt; GSM38757     3  0.0000      0.962 0.000 0.000 1.000
#&gt; GSM38758     2  0.0000      0.994 0.000 1.000 0.000
#&gt; GSM38759     3  0.1643      0.938 0.044 0.000 0.956
#&gt; GSM38760     1  0.0592      0.936 0.988 0.000 0.012
#&gt; GSM38761     2  0.0000      0.994 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000      0.994 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.3764      0.744 0.784 0.000 0.216 0.000
#&gt; GSM38714     3  0.0921      0.894 0.028 0.000 0.972 0.000
#&gt; GSM38715     1  0.3610      0.763 0.800 0.000 0.200 0.000
#&gt; GSM38716     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0188      0.958 0.996 0.000 0.004 0.000
#&gt; GSM38719     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38721     3  0.0921      0.894 0.028 0.000 0.972 0.000
#&gt; GSM38722     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.0000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38725     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38728     3  0.3528      0.845 0.000 0.000 0.808 0.192
#&gt; GSM38729     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.0000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38733     1  0.3873      0.736 0.772 0.000 0.228 0.000
#&gt; GSM38734     3  0.3610      0.840 0.000 0.000 0.800 0.200
#&gt; GSM38735     4  0.3610      1.000 0.000 0.200 0.000 0.800
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38738     3  0.0000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38739     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38740     4  0.3610      1.000 0.000 0.200 0.000 0.800
#&gt; GSM38741     3  0.3486      0.847 0.000 0.000 0.812 0.188
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38744     4  0.3610      1.000 0.000 0.200 0.000 0.800
#&gt; GSM38745     4  0.3610      1.000 0.000 0.200 0.000 0.800
#&gt; GSM38746     3  0.0921      0.894 0.028 0.000 0.972 0.000
#&gt; GSM38747     3  0.0921      0.894 0.028 0.000 0.972 0.000
#&gt; GSM38748     3  0.3610      0.840 0.000 0.000 0.800 0.200
#&gt; GSM38749     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38750     3  0.0000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38751     3  0.0000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38752     3  0.3610      0.840 0.000 0.000 0.800 0.200
#&gt; GSM38753     3  0.3764      0.738 0.000 0.216 0.784 0.000
#&gt; GSM38754     3  0.3610      0.840 0.000 0.000 0.800 0.200
#&gt; GSM38755     3  0.0000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38756     3  0.3610      0.755 0.000 0.200 0.800 0.000
#&gt; GSM38757     3  0.0000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38758     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38759     3  0.0921      0.894 0.028 0.000 0.972 0.000
#&gt; GSM38760     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM38712     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38713     1  0.4491      0.652 0.652 0.000 0.020 0.328  0
#&gt; GSM38714     1  0.5353      0.588 0.600 0.000 0.072 0.328  0
#&gt; GSM38715     1  0.4491      0.652 0.652 0.000 0.020 0.328  0
#&gt; GSM38716     1  0.2605      0.788 0.852 0.000 0.000 0.148  0
#&gt; GSM38717     1  0.2605      0.788 0.852 0.000 0.000 0.148  0
#&gt; GSM38718     1  0.2516      0.792 0.860 0.000 0.000 0.140  0
#&gt; GSM38719     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38720     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38721     1  0.5353      0.588 0.600 0.000 0.072 0.328  0
#&gt; GSM38722     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38723     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38724     3  0.3932      0.685 0.000 0.000 0.672 0.328  0
#&gt; GSM38725     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38726     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38727     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38728     3  0.3690      0.466 0.224 0.000 0.764 0.012  0
#&gt; GSM38729     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38730     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38731     1  0.0000      0.840 1.000 0.000 0.000 0.000  0
#&gt; GSM38732     3  0.2929      0.740 0.000 0.000 0.820 0.180  0
#&gt; GSM38733     1  0.5513      0.590 0.652 0.000 0.168 0.180  0
#&gt; GSM38734     3  0.0609      0.648 0.000 0.000 0.980 0.020  0
#&gt; GSM38735     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000 0.000 0.000  0
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000 0.000 0.000  0
#&gt; GSM38738     3  0.2929      0.740 0.000 0.000 0.820 0.180  0
#&gt; GSM38739     3  0.4182      0.432 0.400 0.000 0.600 0.000  0
#&gt; GSM38740     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM38741     3  0.2690      0.639 0.000 0.000 0.844 0.156  0
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000 0.000 0.000  0
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000 0.000 0.000  0
#&gt; GSM38744     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM38745     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM38746     3  0.3932      0.685 0.000 0.000 0.672 0.328  0
#&gt; GSM38747     3  0.6638      0.424 0.236 0.000 0.436 0.328  0
#&gt; GSM38748     4  0.3932      0.519 0.000 0.000 0.328 0.672  0
#&gt; GSM38749     3  0.4242      0.407 0.428 0.000 0.572 0.000  0
#&gt; GSM38750     3  0.2929      0.740 0.000 0.000 0.820 0.180  0
#&gt; GSM38751     3  0.3932      0.685 0.000 0.000 0.672 0.328  0
#&gt; GSM38752     3  0.0609      0.648 0.000 0.000 0.980 0.020  0
#&gt; GSM38753     4  0.3274      0.736 0.000 0.220 0.000 0.780  0
#&gt; GSM38754     3  0.0609      0.648 0.000 0.000 0.980 0.020  0
#&gt; GSM38755     3  0.2929      0.740 0.000 0.000 0.820 0.180  0
#&gt; GSM38756     4  0.3395      0.731 0.000 0.236 0.000 0.764  0
#&gt; GSM38757     3  0.2929      0.740 0.000 0.000 0.820 0.180  0
#&gt; GSM38758     2  0.0000      1.000 0.000 1.000 0.000 0.000  0
#&gt; GSM38759     1  0.5353      0.588 0.600 0.000 0.072 0.328  0
#&gt; GSM38760     1  0.1121      0.808 0.956 0.000 0.044 0.000  0
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000 0.000 0.000  0
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000 0.000 0.000  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1  p2    p3    p4 p5    p6
#&gt; GSM38712     1  0.0146      0.937 0.996 0.0 0.000 0.000  0 0.004
#&gt; GSM38713     6  0.0146      0.871 0.004 0.0 0.000 0.000  0 0.996
#&gt; GSM38714     6  0.0000      0.871 0.000 0.0 0.000 0.000  0 1.000
#&gt; GSM38715     6  0.0146      0.871 0.004 0.0 0.000 0.000  0 0.996
#&gt; GSM38716     6  0.2854      0.713 0.208 0.0 0.000 0.000  0 0.792
#&gt; GSM38717     6  0.0632      0.865 0.024 0.0 0.000 0.000  0 0.976
#&gt; GSM38718     1  0.3797      0.313 0.580 0.0 0.000 0.000  0 0.420
#&gt; GSM38719     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38720     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38721     6  0.0000      0.871 0.000 0.0 0.000 0.000  0 1.000
#&gt; GSM38722     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38723     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38724     6  0.0146      0.871 0.000 0.0 0.004 0.000  0 0.996
#&gt; GSM38725     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38726     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38727     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38728     3  0.0146      0.837 0.000 0.0 0.996 0.000  0 0.004
#&gt; GSM38729     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38730     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38731     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38732     3  0.3522      0.816 0.000 0.0 0.800 0.072  0 0.128
#&gt; GSM38733     3  0.2823      0.754 0.000 0.0 0.796 0.000  0 0.204
#&gt; GSM38734     3  0.0000      0.839 0.000 0.0 1.000 0.000  0 0.000
#&gt; GSM38735     5  0.0000      1.000 0.000 0.0 0.000 0.000  1 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000 1.0 0.000 0.000  0 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.0 0.000 0.000  0 0.000
#&gt; GSM38738     3  0.2793      0.847 0.000 0.0 0.800 0.200  0 0.000
#&gt; GSM38739     1  0.2668      0.780 0.828 0.0 0.004 0.168  0 0.000
#&gt; GSM38740     5  0.0000      1.000 0.000 0.0 0.000 0.000  1 0.000
#&gt; GSM38741     6  0.2823      0.738 0.000 0.0 0.204 0.000  0 0.796
#&gt; GSM38742     2  0.0000      1.000 0.000 1.0 0.000 0.000  0 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.0 0.000 0.000  0 0.000
#&gt; GSM38744     5  0.0000      1.000 0.000 0.0 0.000 0.000  1 0.000
#&gt; GSM38745     5  0.0000      1.000 0.000 0.0 0.000 0.000  1 0.000
#&gt; GSM38746     6  0.2933      0.770 0.000 0.0 0.004 0.200  0 0.796
#&gt; GSM38747     6  0.5308      0.585 0.180 0.0 0.004 0.200  0 0.616
#&gt; GSM38748     4  0.2793      0.696 0.000 0.0 0.200 0.800  0 0.000
#&gt; GSM38749     1  0.2320      0.820 0.864 0.0 0.004 0.132  0 0.000
#&gt; GSM38750     3  0.2933      0.846 0.000 0.0 0.796 0.200  0 0.004
#&gt; GSM38751     6  0.3043      0.767 0.000 0.0 0.008 0.200  0 0.792
#&gt; GSM38752     3  0.0000      0.839 0.000 0.0 1.000 0.000  0 0.000
#&gt; GSM38753     4  0.2793      0.847 0.000 0.2 0.000 0.800  0 0.000
#&gt; GSM38754     3  0.0000      0.839 0.000 0.0 1.000 0.000  0 0.000
#&gt; GSM38755     3  0.2933      0.846 0.000 0.0 0.796 0.200  0 0.004
#&gt; GSM38756     4  0.2793      0.847 0.000 0.2 0.000 0.800  0 0.000
#&gt; GSM38757     3  0.2793      0.847 0.000 0.0 0.800 0.200  0 0.000
#&gt; GSM38758     2  0.0000      1.000 0.000 1.0 0.000 0.000  0 0.000
#&gt; GSM38759     6  0.0000      0.871 0.000 0.0 0.000 0.000  0 1.000
#&gt; GSM38760     1  0.0000      0.940 1.000 0.0 0.000 0.000  0 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000 1.0 0.000 0.000  0 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.0 0.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n individual(p) k
#> CV:pam 43       0.01685 2
#> CV:pam 51       0.00390 3
#> CV:pam 51       0.00110 4
#> CV:pam 47       0.00316 5
#> CV:pam 50       0.00323 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.983       0.993         0.3945 0.613   0.613
#> 3 3 0.624           0.815       0.893         0.6261 0.708   0.531
#> 4 4 0.801           0.859       0.901         0.0926 0.962   0.890
#> 5 5 0.675           0.795       0.862         0.0458 0.952   0.850
#> 6 6 0.748           0.738       0.839         0.0796 0.909   0.688
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1   p2
#&gt; GSM38712     1   0.000      0.991 1.00 0.00
#&gt; GSM38713     1   0.000      0.991 1.00 0.00
#&gt; GSM38714     1   0.000      0.991 1.00 0.00
#&gt; GSM38715     1   0.000      0.991 1.00 0.00
#&gt; GSM38716     1   0.000      0.991 1.00 0.00
#&gt; GSM38717     1   0.000      0.991 1.00 0.00
#&gt; GSM38718     1   0.000      0.991 1.00 0.00
#&gt; GSM38719     1   0.000      0.991 1.00 0.00
#&gt; GSM38720     1   0.000      0.991 1.00 0.00
#&gt; GSM38721     1   0.000      0.991 1.00 0.00
#&gt; GSM38722     1   0.000      0.991 1.00 0.00
#&gt; GSM38723     1   0.000      0.991 1.00 0.00
#&gt; GSM38724     1   0.000      0.991 1.00 0.00
#&gt; GSM38725     1   0.000      0.991 1.00 0.00
#&gt; GSM38726     1   0.000      0.991 1.00 0.00
#&gt; GSM38727     1   0.000      0.991 1.00 0.00
#&gt; GSM38728     1   0.000      0.991 1.00 0.00
#&gt; GSM38729     1   0.000      0.991 1.00 0.00
#&gt; GSM38730     1   0.000      0.991 1.00 0.00
#&gt; GSM38731     1   0.000      0.991 1.00 0.00
#&gt; GSM38732     1   0.000      0.991 1.00 0.00
#&gt; GSM38733     1   0.000      0.991 1.00 0.00
#&gt; GSM38734     1   0.000      0.991 1.00 0.00
#&gt; GSM38735     2   0.000      1.000 0.00 1.00
#&gt; GSM38736     2   0.000      1.000 0.00 1.00
#&gt; GSM38737     2   0.000      1.000 0.00 1.00
#&gt; GSM38738     1   0.000      0.991 1.00 0.00
#&gt; GSM38739     1   0.000      0.991 1.00 0.00
#&gt; GSM38740     2   0.000      1.000 0.00 1.00
#&gt; GSM38741     1   0.000      0.991 1.00 0.00
#&gt; GSM38742     2   0.000      1.000 0.00 1.00
#&gt; GSM38743     2   0.000      1.000 0.00 1.00
#&gt; GSM38744     2   0.000      1.000 0.00 1.00
#&gt; GSM38745     2   0.000      1.000 0.00 1.00
#&gt; GSM38746     1   0.000      0.991 1.00 0.00
#&gt; GSM38747     1   0.000      0.991 1.00 0.00
#&gt; GSM38748     1   0.925      0.485 0.66 0.34
#&gt; GSM38749     1   0.000      0.991 1.00 0.00
#&gt; GSM38750     1   0.000      0.991 1.00 0.00
#&gt; GSM38751     1   0.000      0.991 1.00 0.00
#&gt; GSM38752     1   0.000      0.991 1.00 0.00
#&gt; GSM38753     2   0.000      1.000 0.00 1.00
#&gt; GSM38754     1   0.000      0.991 1.00 0.00
#&gt; GSM38755     1   0.000      0.991 1.00 0.00
#&gt; GSM38756     2   0.000      1.000 0.00 1.00
#&gt; GSM38757     1   0.000      0.991 1.00 0.00
#&gt; GSM38758     2   0.000      1.000 0.00 1.00
#&gt; GSM38759     1   0.000      0.991 1.00 0.00
#&gt; GSM38760     1   0.000      0.991 1.00 0.00
#&gt; GSM38761     2   0.000      1.000 0.00 1.00
#&gt; GSM38762     2   0.000      1.000 0.00 1.00
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.837 1.000 0.000 0.000
#&gt; GSM38713     1  0.0237      0.837 0.996 0.000 0.004
#&gt; GSM38714     1  0.2959      0.803 0.900 0.000 0.100
#&gt; GSM38715     1  0.0237      0.837 0.996 0.000 0.004
#&gt; GSM38716     1  0.4121      0.769 0.832 0.000 0.168
#&gt; GSM38717     1  0.0237      0.838 0.996 0.000 0.004
#&gt; GSM38718     1  0.0000      0.837 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.837 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.837 1.000 0.000 0.000
#&gt; GSM38721     1  0.4750      0.644 0.784 0.000 0.216
#&gt; GSM38722     1  0.4062      0.772 0.836 0.000 0.164
#&gt; GSM38723     1  0.4062      0.772 0.836 0.000 0.164
#&gt; GSM38724     1  0.6260      0.256 0.552 0.000 0.448
#&gt; GSM38725     1  0.2261      0.825 0.932 0.000 0.068
#&gt; GSM38726     1  0.1289      0.833 0.968 0.000 0.032
#&gt; GSM38727     1  0.4062      0.772 0.836 0.000 0.164
#&gt; GSM38728     3  0.6026      0.615 0.376 0.000 0.624
#&gt; GSM38729     1  0.1860      0.832 0.948 0.000 0.052
#&gt; GSM38730     1  0.0000      0.837 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.837 1.000 0.000 0.000
#&gt; GSM38732     3  0.4654      0.857 0.208 0.000 0.792
#&gt; GSM38733     1  0.4750      0.644 0.784 0.000 0.216
#&gt; GSM38734     3  0.1163      0.785 0.028 0.000 0.972
#&gt; GSM38735     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38736     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38738     3  0.4235      0.880 0.176 0.000 0.824
#&gt; GSM38739     3  0.4605      0.860 0.204 0.000 0.796
#&gt; GSM38740     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38741     3  0.0592      0.791 0.012 0.000 0.988
#&gt; GSM38742     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38744     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38745     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38746     3  0.4235      0.880 0.176 0.000 0.824
#&gt; GSM38747     3  0.4605      0.860 0.204 0.000 0.796
#&gt; GSM38748     2  0.6540      0.424 0.008 0.584 0.408
#&gt; GSM38749     3  0.4235      0.880 0.176 0.000 0.824
#&gt; GSM38750     3  0.4235      0.880 0.176 0.000 0.824
#&gt; GSM38751     3  0.4235      0.880 0.176 0.000 0.824
#&gt; GSM38752     3  0.1289      0.788 0.032 0.000 0.968
#&gt; GSM38753     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38754     3  0.1163      0.785 0.028 0.000 0.972
#&gt; GSM38755     1  0.6260      0.256 0.552 0.000 0.448
#&gt; GSM38756     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38757     3  0.4235      0.880 0.176 0.000 0.824
#&gt; GSM38758     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38759     1  0.6168      0.499 0.588 0.000 0.412
#&gt; GSM38760     1  0.4931      0.705 0.768 0.000 0.232
#&gt; GSM38761     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000      0.970 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0336      0.914 0.992 0.008 0.000 0.000
#&gt; GSM38713     1  0.0376      0.913 0.992 0.004 0.004 0.000
#&gt; GSM38714     1  0.0712      0.910 0.984 0.004 0.004 0.008
#&gt; GSM38715     1  0.0376      0.913 0.992 0.004 0.004 0.000
#&gt; GSM38716     1  0.3774      0.870 0.844 0.128 0.020 0.008
#&gt; GSM38717     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0188      0.914 0.996 0.000 0.004 0.000
#&gt; GSM38720     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.0188      0.914 0.996 0.000 0.004 0.000
#&gt; GSM38722     1  0.4194      0.846 0.800 0.172 0.028 0.000
#&gt; GSM38723     1  0.4514      0.846 0.800 0.136 0.064 0.000
#&gt; GSM38724     1  0.4606      0.704 0.724 0.012 0.264 0.000
#&gt; GSM38725     1  0.1256      0.911 0.964 0.028 0.008 0.000
#&gt; GSM38726     1  0.0469      0.913 0.988 0.012 0.000 0.000
#&gt; GSM38727     1  0.4194      0.846 0.800 0.172 0.028 0.000
#&gt; GSM38728     3  0.3726      0.752 0.212 0.000 0.788 0.000
#&gt; GSM38729     1  0.2530      0.883 0.888 0.112 0.000 0.000
#&gt; GSM38730     1  0.0469      0.913 0.988 0.012 0.000 0.000
#&gt; GSM38731     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.2197      0.923 0.080 0.004 0.916 0.000
#&gt; GSM38733     1  0.0188      0.914 0.996 0.000 0.004 0.000
#&gt; GSM38734     3  0.1624      0.935 0.020 0.028 0.952 0.000
#&gt; GSM38735     4  0.1302      0.805 0.000 0.044 0.000 0.956
#&gt; GSM38736     2  0.3688      1.000 0.000 0.792 0.000 0.208
#&gt; GSM38737     2  0.3688      1.000 0.000 0.792 0.000 0.208
#&gt; GSM38738     3  0.1284      0.949 0.024 0.012 0.964 0.000
#&gt; GSM38739     3  0.0817      0.950 0.024 0.000 0.976 0.000
#&gt; GSM38740     4  0.1302      0.805 0.000 0.044 0.000 0.956
#&gt; GSM38741     3  0.0937      0.946 0.012 0.012 0.976 0.000
#&gt; GSM38742     2  0.3688      1.000 0.000 0.792 0.000 0.208
#&gt; GSM38743     2  0.3688      1.000 0.000 0.792 0.000 0.208
#&gt; GSM38744     4  0.1302      0.805 0.000 0.044 0.000 0.956
#&gt; GSM38745     4  0.1302      0.805 0.000 0.044 0.000 0.956
#&gt; GSM38746     3  0.1389      0.936 0.048 0.000 0.952 0.000
#&gt; GSM38747     3  0.1474      0.936 0.052 0.000 0.948 0.000
#&gt; GSM38748     4  0.5910      0.372 0.008 0.040 0.316 0.636
#&gt; GSM38749     3  0.0817      0.950 0.024 0.000 0.976 0.000
#&gt; GSM38750     3  0.0817      0.950 0.024 0.000 0.976 0.000
#&gt; GSM38751     3  0.0779      0.948 0.016 0.004 0.980 0.000
#&gt; GSM38752     3  0.1624      0.935 0.020 0.028 0.952 0.000
#&gt; GSM38753     4  0.0000      0.793 0.000 0.000 0.000 1.000
#&gt; GSM38754     3  0.1624      0.935 0.020 0.028 0.952 0.000
#&gt; GSM38755     1  0.5414      0.471 0.604 0.020 0.376 0.000
#&gt; GSM38756     4  0.0000      0.793 0.000 0.000 0.000 1.000
#&gt; GSM38757     3  0.1284      0.949 0.024 0.012 0.964 0.000
#&gt; GSM38758     2  0.3688      1.000 0.000 0.792 0.000 0.208
#&gt; GSM38759     1  0.4424      0.852 0.808 0.148 0.036 0.008
#&gt; GSM38760     1  0.4121      0.798 0.796 0.020 0.184 0.000
#&gt; GSM38761     2  0.3688      1.000 0.000 0.792 0.000 0.208
#&gt; GSM38762     4  0.4981     -0.262 0.000 0.464 0.000 0.536
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.2127      0.853 0.892 0.108 0.000 0.000 0.000
#&gt; GSM38713     1  0.0162      0.861 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38714     1  0.0162      0.861 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38715     1  0.0162      0.861 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38716     1  0.2020      0.852 0.900 0.100 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.861 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.861 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.861 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.861 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.3840      0.686 0.780 0.016 0.196 0.008 0.000
#&gt; GSM38722     1  0.4832      0.781 0.712 0.200 0.000 0.088 0.000
#&gt; GSM38723     1  0.4883      0.778 0.708 0.200 0.000 0.092 0.000
#&gt; GSM38724     1  0.7072      0.249 0.464 0.112 0.364 0.060 0.000
#&gt; GSM38725     1  0.2813      0.835 0.832 0.168 0.000 0.000 0.000
#&gt; GSM38726     1  0.2179      0.852 0.888 0.112 0.000 0.000 0.000
#&gt; GSM38727     1  0.4883      0.778 0.708 0.200 0.000 0.092 0.000
#&gt; GSM38728     3  0.4069      0.758 0.088 0.004 0.800 0.108 0.000
#&gt; GSM38729     1  0.1851      0.846 0.912 0.000 0.000 0.088 0.000
#&gt; GSM38730     1  0.2179      0.852 0.888 0.112 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.861 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.4358      0.771 0.052 0.040 0.800 0.108 0.000
#&gt; GSM38733     1  0.3840      0.686 0.780 0.016 0.196 0.008 0.000
#&gt; GSM38734     3  0.3366      0.748 0.000 0.000 0.768 0.232 0.000
#&gt; GSM38735     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38736     2  0.3109      1.000 0.000 0.800 0.000 0.000 0.200
#&gt; GSM38737     2  0.3109      1.000 0.000 0.800 0.000 0.000 0.200
#&gt; GSM38738     3  0.3216      0.779 0.000 0.044 0.848 0.108 0.000
#&gt; GSM38739     3  0.3074      0.617 0.196 0.000 0.804 0.000 0.000
#&gt; GSM38740     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38741     3  0.3276      0.778 0.000 0.032 0.836 0.132 0.000
#&gt; GSM38742     2  0.3109      1.000 0.000 0.800 0.000 0.000 0.200
#&gt; GSM38743     2  0.3109      1.000 0.000 0.800 0.000 0.000 0.200
#&gt; GSM38744     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38745     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38746     3  0.2179      0.712 0.112 0.000 0.888 0.000 0.000
#&gt; GSM38747     3  0.0000      0.779 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38748     4  0.2172      0.504 0.000 0.000 0.076 0.908 0.016
#&gt; GSM38749     3  0.3074      0.617 0.196 0.000 0.804 0.000 0.000
#&gt; GSM38750     3  0.0000      0.779 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38751     3  0.0162      0.779 0.004 0.000 0.996 0.000 0.000
#&gt; GSM38752     3  0.3366      0.748 0.000 0.000 0.768 0.232 0.000
#&gt; GSM38753     4  0.3913      0.664 0.000 0.000 0.000 0.676 0.324
#&gt; GSM38754     3  0.3366      0.748 0.000 0.000 0.768 0.232 0.000
#&gt; GSM38755     3  0.6488     -0.213 0.408 0.160 0.428 0.004 0.000
#&gt; GSM38756     4  0.3913      0.664 0.000 0.000 0.000 0.676 0.324
#&gt; GSM38757     3  0.0404      0.781 0.000 0.012 0.988 0.000 0.000
#&gt; GSM38758     2  0.3109      1.000 0.000 0.800 0.000 0.000 0.200
#&gt; GSM38759     1  0.3323      0.811 0.844 0.000 0.056 0.100 0.000
#&gt; GSM38760     1  0.5699      0.764 0.696 0.168 0.064 0.072 0.000
#&gt; GSM38761     2  0.3109      1.000 0.000 0.800 0.000 0.000 0.200
#&gt; GSM38762     2  0.3109      1.000 0.000 0.800 0.000 0.000 0.200
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1387      0.856 0.932 0.000 0.000 0.000 0.000 0.068
#&gt; GSM38713     6  0.3448      0.571 0.280 0.000 0.004 0.000 0.000 0.716
#&gt; GSM38714     6  0.3769      0.454 0.356 0.000 0.000 0.000 0.004 0.640
#&gt; GSM38715     6  0.3756      0.340 0.400 0.000 0.000 0.000 0.000 0.600
#&gt; GSM38716     1  0.2320      0.832 0.864 0.000 0.000 0.000 0.004 0.132
#&gt; GSM38717     1  0.2805      0.789 0.812 0.000 0.000 0.000 0.004 0.184
#&gt; GSM38718     1  0.3076      0.616 0.760 0.000 0.000 0.000 0.000 0.240
#&gt; GSM38719     1  0.2772      0.794 0.816 0.000 0.000 0.000 0.004 0.180
#&gt; GSM38720     1  0.1152      0.855 0.952 0.000 0.000 0.000 0.004 0.044
#&gt; GSM38721     6  0.1895      0.631 0.072 0.000 0.016 0.000 0.000 0.912
#&gt; GSM38722     1  0.0993      0.834 0.964 0.000 0.000 0.012 0.000 0.024
#&gt; GSM38723     1  0.1563      0.813 0.932 0.000 0.000 0.012 0.000 0.056
#&gt; GSM38724     6  0.4469      0.360 0.012 0.000 0.088 0.172 0.000 0.728
#&gt; GSM38725     1  0.1814      0.847 0.900 0.000 0.000 0.000 0.000 0.100
#&gt; GSM38726     1  0.1204      0.857 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM38727     1  0.1563      0.813 0.932 0.000 0.000 0.012 0.000 0.056
#&gt; GSM38728     3  0.2692      0.680 0.012 0.000 0.840 0.000 0.000 0.148
#&gt; GSM38729     1  0.0935      0.841 0.964 0.000 0.000 0.000 0.004 0.032
#&gt; GSM38730     1  0.2092      0.836 0.876 0.000 0.000 0.000 0.000 0.124
#&gt; GSM38731     1  0.0508      0.845 0.984 0.000 0.000 0.000 0.004 0.012
#&gt; GSM38732     3  0.3652      0.618 0.004 0.000 0.672 0.000 0.000 0.324
#&gt; GSM38733     6  0.1895      0.631 0.072 0.000 0.016 0.000 0.000 0.912
#&gt; GSM38734     3  0.0260      0.592 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM38735     5  0.0146      1.000 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38738     3  0.4868      0.706 0.004 0.000 0.676 0.172 0.000 0.148
#&gt; GSM38739     3  0.7038      0.448 0.092 0.000 0.396 0.180 0.000 0.332
#&gt; GSM38740     5  0.0146      1.000 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM38741     3  0.2340      0.688 0.000 0.000 0.852 0.000 0.000 0.148
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.0146      1.000 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM38745     5  0.0146      1.000 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM38746     3  0.5774      0.564 0.000 0.000 0.456 0.180 0.000 0.364
#&gt; GSM38747     3  0.5921      0.537 0.004 0.000 0.432 0.180 0.000 0.384
#&gt; GSM38748     4  0.3198      0.650 0.000 0.000 0.260 0.740 0.000 0.000
#&gt; GSM38749     3  0.6946      0.451 0.080 0.000 0.396 0.180 0.000 0.344
#&gt; GSM38750     3  0.4825      0.706 0.000 0.000 0.668 0.180 0.000 0.152
#&gt; GSM38751     3  0.5774      0.564 0.000 0.000 0.456 0.180 0.000 0.364
#&gt; GSM38752     3  0.0260      0.592 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM38753     4  0.2793      0.825 0.000 0.200 0.000 0.800 0.000 0.000
#&gt; GSM38754     3  0.0260      0.592 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM38755     6  0.4516      0.349 0.012 0.000 0.092 0.172 0.000 0.724
#&gt; GSM38756     4  0.2793      0.825 0.000 0.200 0.000 0.800 0.000 0.000
#&gt; GSM38757     3  0.4795      0.707 0.000 0.000 0.672 0.176 0.000 0.152
#&gt; GSM38758     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38759     1  0.4462      0.295 0.572 0.000 0.012 0.008 0.004 0.404
#&gt; GSM38760     1  0.2632      0.809 0.832 0.000 0.004 0.000 0.000 0.164
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n individual(p) k
#> CV:mclust 50       0.01498 2
#> CV:mclust 47       0.00289 3
#> CV:mclust 48       0.00317 4
#> CV:mclust 49       0.00435 5
#> CV:mclust 44       0.03189 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.728           0.845       0.938          0.465 0.547   0.547
#> 3 3 0.570           0.765       0.871          0.343 0.742   0.564
#> 4 4 0.539           0.499       0.748          0.123 0.940   0.844
#> 5 5 0.668           0.678       0.825          0.101 0.829   0.527
#> 6 6 0.702           0.529       0.779          0.044 0.951   0.799
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.923 1.000 0.000
#&gt; GSM38713     1  0.0000      0.923 1.000 0.000
#&gt; GSM38714     1  0.0000      0.923 1.000 0.000
#&gt; GSM38715     1  0.0000      0.923 1.000 0.000
#&gt; GSM38716     1  0.0000      0.923 1.000 0.000
#&gt; GSM38717     1  0.0000      0.923 1.000 0.000
#&gt; GSM38718     1  0.0000      0.923 1.000 0.000
#&gt; GSM38719     1  0.0000      0.923 1.000 0.000
#&gt; GSM38720     1  0.0000      0.923 1.000 0.000
#&gt; GSM38721     1  0.0376      0.921 0.996 0.004
#&gt; GSM38722     1  0.0000      0.923 1.000 0.000
#&gt; GSM38723     1  0.0000      0.923 1.000 0.000
#&gt; GSM38724     1  0.6148      0.788 0.848 0.152
#&gt; GSM38725     1  0.0000      0.923 1.000 0.000
#&gt; GSM38726     1  0.0000      0.923 1.000 0.000
#&gt; GSM38727     1  0.0000      0.923 1.000 0.000
#&gt; GSM38728     2  0.9833      0.213 0.424 0.576
#&gt; GSM38729     1  0.0000      0.923 1.000 0.000
#&gt; GSM38730     1  0.0000      0.923 1.000 0.000
#&gt; GSM38731     1  0.0000      0.923 1.000 0.000
#&gt; GSM38732     2  0.5737      0.805 0.136 0.864
#&gt; GSM38733     1  0.0000      0.923 1.000 0.000
#&gt; GSM38734     2  0.0000      0.939 0.000 1.000
#&gt; GSM38735     1  0.8207      0.633 0.744 0.256
#&gt; GSM38736     2  0.0000      0.939 0.000 1.000
#&gt; GSM38737     2  0.0938      0.930 0.012 0.988
#&gt; GSM38738     2  0.8661      0.563 0.288 0.712
#&gt; GSM38739     1  0.0000      0.923 1.000 0.000
#&gt; GSM38740     1  0.0000      0.923 1.000 0.000
#&gt; GSM38741     2  0.0000      0.939 0.000 1.000
#&gt; GSM38742     2  0.0000      0.939 0.000 1.000
#&gt; GSM38743     2  0.0000      0.939 0.000 1.000
#&gt; GSM38744     1  0.0000      0.923 1.000 0.000
#&gt; GSM38745     1  0.1184      0.913 0.984 0.016
#&gt; GSM38746     1  0.8661      0.598 0.712 0.288
#&gt; GSM38747     1  0.9491      0.432 0.632 0.368
#&gt; GSM38748     2  0.0000      0.939 0.000 1.000
#&gt; GSM38749     1  0.0000      0.923 1.000 0.000
#&gt; GSM38750     1  0.9983      0.112 0.524 0.476
#&gt; GSM38751     1  0.8443      0.638 0.728 0.272
#&gt; GSM38752     2  0.0000      0.939 0.000 1.000
#&gt; GSM38753     2  0.0000      0.939 0.000 1.000
#&gt; GSM38754     2  0.0000      0.939 0.000 1.000
#&gt; GSM38755     1  0.0000      0.923 1.000 0.000
#&gt; GSM38756     2  0.0000      0.939 0.000 1.000
#&gt; GSM38757     1  0.9815      0.293 0.580 0.420
#&gt; GSM38758     2  0.0000      0.939 0.000 1.000
#&gt; GSM38759     1  0.2948      0.885 0.948 0.052
#&gt; GSM38760     1  0.0000      0.923 1.000 0.000
#&gt; GSM38761     2  0.0000      0.939 0.000 1.000
#&gt; GSM38762     2  0.0000      0.939 0.000 1.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0237     0.8832 0.996 0.004 0.000
#&gt; GSM38713     1  0.1031     0.8753 0.976 0.000 0.024
#&gt; GSM38714     1  0.0892     0.8769 0.980 0.000 0.020
#&gt; GSM38715     1  0.1031     0.8753 0.976 0.000 0.024
#&gt; GSM38716     1  0.0424     0.8832 0.992 0.008 0.000
#&gt; GSM38717     1  0.0237     0.8832 0.996 0.004 0.000
#&gt; GSM38718     1  0.0000     0.8827 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000     0.8827 1.000 0.000 0.000
#&gt; GSM38720     1  0.0237     0.8832 0.996 0.004 0.000
#&gt; GSM38721     1  0.1031     0.8753 0.976 0.000 0.024
#&gt; GSM38722     1  0.4235     0.8240 0.824 0.176 0.000
#&gt; GSM38723     1  0.4235     0.8240 0.824 0.176 0.000
#&gt; GSM38724     3  0.5905     0.4712 0.352 0.000 0.648
#&gt; GSM38725     1  0.0000     0.8827 1.000 0.000 0.000
#&gt; GSM38726     1  0.0592     0.8830 0.988 0.012 0.000
#&gt; GSM38727     1  0.4235     0.8240 0.824 0.176 0.000
#&gt; GSM38728     3  0.5327     0.5730 0.272 0.000 0.728
#&gt; GSM38729     1  0.0424     0.8832 0.992 0.008 0.000
#&gt; GSM38730     1  0.2625     0.8644 0.916 0.084 0.000
#&gt; GSM38731     1  0.2448     0.8672 0.924 0.076 0.000
#&gt; GSM38732     3  0.4002     0.6921 0.160 0.000 0.840
#&gt; GSM38733     1  0.1031     0.8753 0.976 0.000 0.024
#&gt; GSM38734     3  0.0237     0.7949 0.004 0.000 0.996
#&gt; GSM38735     2  0.1031     0.7868 0.024 0.976 0.000
#&gt; GSM38736     2  0.4235     0.8450 0.000 0.824 0.176
#&gt; GSM38737     2  0.4235     0.8450 0.000 0.824 0.176
#&gt; GSM38738     3  0.0424     0.7959 0.008 0.000 0.992
#&gt; GSM38739     1  0.4235     0.8240 0.824 0.176 0.000
#&gt; GSM38740     2  0.1031     0.7868 0.024 0.976 0.000
#&gt; GSM38741     3  0.1031     0.7908 0.000 0.024 0.976
#&gt; GSM38742     2  0.4235     0.8450 0.000 0.824 0.176
#&gt; GSM38743     2  0.4235     0.8450 0.000 0.824 0.176
#&gt; GSM38744     2  0.1031     0.7868 0.024 0.976 0.000
#&gt; GSM38745     2  0.1031     0.7868 0.024 0.976 0.000
#&gt; GSM38746     1  0.5911     0.7352 0.784 0.060 0.156
#&gt; GSM38747     1  0.5331     0.7128 0.792 0.024 0.184
#&gt; GSM38748     3  0.0000     0.7951 0.000 0.000 1.000
#&gt; GSM38749     1  0.4235     0.8240 0.824 0.176 0.000
#&gt; GSM38750     3  0.6977     0.6377 0.212 0.076 0.712
#&gt; GSM38751     1  0.9433    -0.0485 0.420 0.176 0.404
#&gt; GSM38752     3  0.0424     0.7950 0.000 0.008 0.992
#&gt; GSM38753     3  0.4974     0.5628 0.000 0.236 0.764
#&gt; GSM38754     3  0.1031     0.7908 0.000 0.024 0.976
#&gt; GSM38755     1  0.8260    -0.0151 0.492 0.076 0.432
#&gt; GSM38756     3  0.1031     0.7908 0.000 0.024 0.976
#&gt; GSM38757     3  0.5058     0.6533 0.244 0.000 0.756
#&gt; GSM38758     2  0.6280     0.4104 0.000 0.540 0.460
#&gt; GSM38759     1  0.1129     0.8756 0.976 0.020 0.004
#&gt; GSM38760     1  0.4235     0.8240 0.824 0.176 0.000
#&gt; GSM38761     2  0.4235     0.8450 0.000 0.824 0.176
#&gt; GSM38762     2  0.4235     0.8450 0.000 0.824 0.176
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0188    0.71019 0.996 0.004 0.000 0.000
#&gt; GSM38713     1  0.0592    0.70793 0.984 0.000 0.016 0.000
#&gt; GSM38714     1  0.0817    0.70363 0.976 0.000 0.024 0.000
#&gt; GSM38715     1  0.0188    0.70898 0.996 0.000 0.004 0.000
#&gt; GSM38716     1  0.2469    0.63394 0.892 0.000 0.108 0.000
#&gt; GSM38717     1  0.0592    0.70709 0.984 0.000 0.016 0.000
#&gt; GSM38718     1  0.2216    0.67452 0.908 0.000 0.092 0.000
#&gt; GSM38719     1  0.0817    0.70363 0.976 0.000 0.024 0.000
#&gt; GSM38720     1  0.0188    0.70901 0.996 0.000 0.004 0.000
#&gt; GSM38721     1  0.2973    0.63663 0.856 0.000 0.144 0.000
#&gt; GSM38722     1  0.6159    0.44958 0.672 0.196 0.132 0.000
#&gt; GSM38723     1  0.7479    0.10135 0.480 0.196 0.324 0.000
#&gt; GSM38724     4  0.7977    0.00328 0.280 0.004 0.304 0.412
#&gt; GSM38725     1  0.1913    0.70274 0.940 0.040 0.020 0.000
#&gt; GSM38726     1  0.2450    0.68677 0.912 0.016 0.072 0.000
#&gt; GSM38727     1  0.6790    0.34265 0.608 0.196 0.196 0.000
#&gt; GSM38728     4  0.7776   -0.23328 0.248 0.000 0.340 0.412
#&gt; GSM38729     1  0.1022    0.69943 0.968 0.000 0.032 0.000
#&gt; GSM38730     1  0.1406    0.70616 0.960 0.016 0.024 0.000
#&gt; GSM38731     1  0.1211    0.70762 0.960 0.000 0.040 0.000
#&gt; GSM38732     4  0.4501    0.53661 0.024 0.000 0.212 0.764
#&gt; GSM38733     1  0.4158    0.53414 0.768 0.000 0.224 0.008
#&gt; GSM38734     4  0.4382    0.57062 0.000 0.000 0.296 0.704
#&gt; GSM38735     2  0.3037    0.65615 0.020 0.880 0.100 0.000
#&gt; GSM38736     2  0.3726    0.75015 0.000 0.788 0.000 0.212
#&gt; GSM38737     2  0.3610    0.75339 0.000 0.800 0.000 0.200
#&gt; GSM38738     4  0.5161    0.50087 0.024 0.000 0.300 0.676
#&gt; GSM38739     1  0.7793    0.05655 0.464 0.196 0.332 0.008
#&gt; GSM38740     2  0.5998    0.47357 0.200 0.684 0.116 0.000
#&gt; GSM38741     4  0.0921    0.59571 0.000 0.000 0.028 0.972
#&gt; GSM38742     2  0.3610    0.75339 0.000 0.800 0.000 0.200
#&gt; GSM38743     2  0.3569    0.75338 0.000 0.804 0.000 0.196
#&gt; GSM38744     2  0.5839    0.48227 0.200 0.696 0.104 0.000
#&gt; GSM38745     2  0.3674    0.64184 0.044 0.852 0.104 0.000
#&gt; GSM38746     1  0.8771   -0.16774 0.408 0.168 0.356 0.068
#&gt; GSM38747     1  0.8195   -0.02675 0.452 0.156 0.356 0.036
#&gt; GSM38748     4  0.5158    0.53160 0.000 0.004 0.472 0.524
#&gt; GSM38749     1  0.7667    0.07038 0.468 0.196 0.332 0.004
#&gt; GSM38750     3  0.9630    0.53988 0.172 0.172 0.352 0.304
#&gt; GSM38751     3  0.9764    0.37067 0.292 0.196 0.332 0.180
#&gt; GSM38752     4  0.0000    0.59170 0.000 0.000 0.000 1.000
#&gt; GSM38753     4  0.6498    0.38540 0.000 0.072 0.440 0.488
#&gt; GSM38754     4  0.0000    0.59170 0.000 0.000 0.000 1.000
#&gt; GSM38755     3  0.8458   -0.17364 0.224 0.032 0.428 0.316
#&gt; GSM38756     4  0.4872    0.51489 0.000 0.004 0.356 0.640
#&gt; GSM38757     3  0.8560    0.41771 0.204 0.040 0.404 0.352
#&gt; GSM38758     2  0.4679    0.60854 0.000 0.648 0.000 0.352
#&gt; GSM38759     1  0.3432    0.59117 0.848 0.004 0.140 0.008
#&gt; GSM38760     1  0.7501    0.08280 0.472 0.196 0.332 0.000
#&gt; GSM38761     2  0.3764    0.74830 0.000 0.784 0.000 0.216
#&gt; GSM38762     2  0.3751    0.63163 0.000 0.800 0.196 0.004
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.1628     0.8616 0.936 0.000 0.056 0.008 0.000
#&gt; GSM38713     1  0.0771     0.8654 0.976 0.000 0.000 0.004 0.020
#&gt; GSM38714     1  0.0451     0.8698 0.988 0.000 0.004 0.008 0.000
#&gt; GSM38715     1  0.0671     0.8664 0.980 0.000 0.000 0.004 0.016
#&gt; GSM38716     1  0.2628     0.8442 0.884 0.000 0.088 0.028 0.000
#&gt; GSM38717     1  0.0162     0.8700 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38718     1  0.0579     0.8702 0.984 0.000 0.008 0.000 0.008
#&gt; GSM38719     1  0.0290     0.8707 0.992 0.000 0.008 0.000 0.000
#&gt; GSM38720     1  0.0451     0.8705 0.988 0.000 0.008 0.004 0.000
#&gt; GSM38721     1  0.3477     0.7748 0.824 0.000 0.000 0.040 0.136
#&gt; GSM38722     3  0.4323     0.4646 0.332 0.000 0.656 0.012 0.000
#&gt; GSM38723     3  0.1410     0.7998 0.060 0.000 0.940 0.000 0.000
#&gt; GSM38724     4  0.8077     0.1541 0.304 0.000 0.092 0.340 0.264
#&gt; GSM38725     1  0.3656     0.7364 0.784 0.000 0.196 0.020 0.000
#&gt; GSM38726     1  0.2329     0.8360 0.876 0.000 0.124 0.000 0.000
#&gt; GSM38727     3  0.2077     0.7861 0.084 0.000 0.908 0.008 0.000
#&gt; GSM38728     1  0.4522     0.3571 0.552 0.000 0.008 0.000 0.440
#&gt; GSM38729     1  0.0693     0.8697 0.980 0.000 0.008 0.012 0.000
#&gt; GSM38730     1  0.2674     0.8204 0.856 0.000 0.140 0.004 0.000
#&gt; GSM38731     1  0.3689     0.6761 0.740 0.000 0.256 0.004 0.000
#&gt; GSM38732     5  0.4176     0.4767 0.108 0.000 0.004 0.096 0.792
#&gt; GSM38733     1  0.4019     0.7417 0.792 0.000 0.004 0.052 0.152
#&gt; GSM38734     5  0.3992     0.4688 0.004 0.000 0.004 0.280 0.712
#&gt; GSM38735     2  0.4497     0.7734 0.016 0.780 0.088 0.116 0.000
#&gt; GSM38736     2  0.0162     0.8318 0.000 0.996 0.000 0.000 0.004
#&gt; GSM38737     2  0.0000     0.8328 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38738     5  0.6060     0.3859 0.004 0.000 0.176 0.228 0.592
#&gt; GSM38739     3  0.1270     0.8014 0.052 0.000 0.948 0.000 0.000
#&gt; GSM38740     2  0.6876     0.6161 0.068 0.572 0.132 0.228 0.000
#&gt; GSM38741     5  0.6122     0.4418 0.000 0.080 0.060 0.216 0.644
#&gt; GSM38742     2  0.0000     0.8328 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000     0.8328 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     2  0.6435     0.6723 0.068 0.636 0.156 0.140 0.000
#&gt; GSM38745     2  0.5337     0.7360 0.024 0.716 0.136 0.124 0.000
#&gt; GSM38746     3  0.2625     0.7452 0.016 0.000 0.900 0.056 0.028
#&gt; GSM38747     3  0.6558     0.3970 0.104 0.028 0.540 0.004 0.324
#&gt; GSM38748     4  0.3790     0.3911 0.004 0.000 0.004 0.744 0.248
#&gt; GSM38749     3  0.1270     0.8014 0.052 0.000 0.948 0.000 0.000
#&gt; GSM38750     3  0.3141     0.6942 0.016 0.000 0.832 0.000 0.152
#&gt; GSM38751     3  0.0727     0.7746 0.004 0.000 0.980 0.012 0.004
#&gt; GSM38752     5  0.3297     0.5864 0.000 0.080 0.048 0.012 0.860
#&gt; GSM38753     4  0.4250     0.4805 0.000 0.084 0.004 0.784 0.128
#&gt; GSM38754     5  0.3366     0.5878 0.004 0.084 0.040 0.012 0.860
#&gt; GSM38755     4  0.5839     0.3451 0.008 0.000 0.192 0.636 0.164
#&gt; GSM38756     4  0.4794     0.4788 0.000 0.080 0.012 0.744 0.164
#&gt; GSM38757     3  0.5225     0.0143 0.008 0.000 0.508 0.028 0.456
#&gt; GSM38758     2  0.3496     0.6594 0.000 0.788 0.012 0.000 0.200
#&gt; GSM38759     1  0.4491     0.7131 0.772 0.008 0.004 0.152 0.064
#&gt; GSM38760     3  0.1430     0.8004 0.052 0.000 0.944 0.000 0.004
#&gt; GSM38761     2  0.0324     0.8302 0.000 0.992 0.004 0.000 0.004
#&gt; GSM38762     2  0.1792     0.7932 0.000 0.916 0.000 0.000 0.084
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1984    0.82202 0.912 0.000 0.032 0.000 0.056 0.000
#&gt; GSM38713     1  0.1666    0.82347 0.936 0.000 0.000 0.020 0.036 0.008
#&gt; GSM38714     1  0.1686    0.82238 0.924 0.000 0.000 0.012 0.064 0.000
#&gt; GSM38715     1  0.1003    0.83007 0.964 0.000 0.000 0.016 0.020 0.000
#&gt; GSM38716     1  0.3560    0.77447 0.808 0.000 0.104 0.000 0.084 0.004
#&gt; GSM38717     1  0.0260    0.83408 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM38718     1  0.0405    0.83417 0.988 0.000 0.000 0.000 0.008 0.004
#&gt; GSM38719     1  0.0000    0.83432 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0653    0.83470 0.980 0.000 0.012 0.000 0.004 0.004
#&gt; GSM38721     1  0.2667    0.76165 0.852 0.000 0.000 0.000 0.020 0.128
#&gt; GSM38722     3  0.4863    0.08893 0.412 0.000 0.528 0.000 0.060 0.000
#&gt; GSM38723     3  0.0692    0.77511 0.020 0.000 0.976 0.000 0.004 0.000
#&gt; GSM38724     4  0.7670    0.21417 0.272 0.000 0.056 0.440 0.100 0.132
#&gt; GSM38725     1  0.5001    0.63170 0.692 0.000 0.176 0.028 0.104 0.000
#&gt; GSM38726     1  0.2488    0.79229 0.864 0.000 0.124 0.000 0.004 0.008
#&gt; GSM38727     3  0.1657    0.75056 0.056 0.000 0.928 0.000 0.016 0.000
#&gt; GSM38728     1  0.7327    0.00225 0.440 0.000 0.004 0.196 0.144 0.216
#&gt; GSM38729     1  0.0806    0.83452 0.972 0.000 0.000 0.008 0.020 0.000
#&gt; GSM38730     1  0.2573    0.79812 0.864 0.000 0.112 0.000 0.024 0.000
#&gt; GSM38731     1  0.3619    0.69041 0.744 0.000 0.232 0.000 0.024 0.000
#&gt; GSM38732     6  0.4511    0.59531 0.060 0.000 0.004 0.048 0.128 0.760
#&gt; GSM38733     6  0.4406    0.39284 0.336 0.000 0.000 0.000 0.040 0.624
#&gt; GSM38734     6  0.0937    0.67437 0.000 0.000 0.000 0.040 0.000 0.960
#&gt; GSM38735     2  0.4527   -0.29545 0.008 0.604 0.028 0.000 0.360 0.000
#&gt; GSM38736     2  0.0291    0.68039 0.000 0.992 0.000 0.004 0.004 0.000
#&gt; GSM38737     2  0.0000    0.68024 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38738     6  0.2214    0.66506 0.000 0.000 0.092 0.012 0.004 0.892
#&gt; GSM38739     3  0.0260    0.77616 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM38740     5  0.6000    0.00000 0.076 0.380 0.056 0.000 0.488 0.000
#&gt; GSM38741     4  0.2339    0.50329 0.000 0.028 0.024 0.908 0.004 0.036
#&gt; GSM38742     2  0.0632    0.68150 0.000 0.976 0.000 0.024 0.000 0.000
#&gt; GSM38743     2  0.0547    0.68268 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; GSM38744     2  0.6081   -0.78963 0.092 0.456 0.048 0.000 0.404 0.000
#&gt; GSM38745     2  0.4853   -0.46866 0.004 0.556 0.052 0.000 0.388 0.000
#&gt; GSM38746     3  0.3952    0.63964 0.000 0.000 0.788 0.088 0.108 0.016
#&gt; GSM38747     3  0.7681   -0.02909 0.016 0.012 0.416 0.292 0.148 0.116
#&gt; GSM38748     6  0.4828    0.36780 0.000 0.000 0.000 0.176 0.156 0.668
#&gt; GSM38749     3  0.0000    0.77688 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38750     3  0.1141    0.76034 0.000 0.000 0.948 0.000 0.000 0.052
#&gt; GSM38751     3  0.0363    0.77539 0.000 0.000 0.988 0.012 0.000 0.000
#&gt; GSM38752     4  0.5291    0.41823 0.000 0.028 0.004 0.676 0.136 0.156
#&gt; GSM38753     4  0.5956    0.43892 0.000 0.028 0.000 0.504 0.348 0.120
#&gt; GSM38754     4  0.5585    0.39381 0.000 0.028 0.004 0.640 0.160 0.168
#&gt; GSM38755     6  0.2706    0.66442 0.000 0.000 0.068 0.024 0.028 0.880
#&gt; GSM38756     4  0.5993    0.43828 0.000 0.028 0.000 0.508 0.336 0.128
#&gt; GSM38757     3  0.4550    0.06524 0.000 0.000 0.524 0.008 0.020 0.448
#&gt; GSM38758     2  0.2704    0.54108 0.000 0.844 0.000 0.140 0.016 0.000
#&gt; GSM38759     1  0.6129    0.08153 0.448 0.000 0.000 0.320 0.224 0.008
#&gt; GSM38760     3  0.0881    0.77644 0.012 0.000 0.972 0.000 0.008 0.008
#&gt; GSM38761     2  0.0713    0.67938 0.000 0.972 0.000 0.028 0.000 0.000
#&gt; GSM38762     2  0.0806    0.65648 0.000 0.972 0.000 0.000 0.008 0.020
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n individual(p) k
#> CV:NMF 47      0.118530 2
#> CV:NMF 47      0.001895 3
#> CV:NMF 35      0.000419 4
#> CV:NMF 38      0.000213 5
#> CV:NMF 35      0.000478 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.979       0.990         0.4220 0.576   0.576
#> 3 3 0.750           0.833       0.910         0.4800 0.784   0.626
#> 4 4 0.725           0.821       0.907         0.0818 0.956   0.878
#> 5 5 0.698           0.800       0.874         0.1189 0.934   0.792
#> 6 6 0.738           0.750       0.844         0.0361 0.988   0.953
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.993 1.000 0.000
#&gt; GSM38713     1   0.000      0.993 1.000 0.000
#&gt; GSM38714     1   0.000      0.993 1.000 0.000
#&gt; GSM38715     1   0.000      0.993 1.000 0.000
#&gt; GSM38716     1   0.000      0.993 1.000 0.000
#&gt; GSM38717     1   0.000      0.993 1.000 0.000
#&gt; GSM38718     1   0.000      0.993 1.000 0.000
#&gt; GSM38719     1   0.000      0.993 1.000 0.000
#&gt; GSM38720     1   0.000      0.993 1.000 0.000
#&gt; GSM38721     1   0.000      0.993 1.000 0.000
#&gt; GSM38722     1   0.000      0.993 1.000 0.000
#&gt; GSM38723     1   0.000      0.993 1.000 0.000
#&gt; GSM38724     1   0.000      0.993 1.000 0.000
#&gt; GSM38725     1   0.000      0.993 1.000 0.000
#&gt; GSM38726     1   0.000      0.993 1.000 0.000
#&gt; GSM38727     1   0.000      0.993 1.000 0.000
#&gt; GSM38728     1   0.260      0.958 0.956 0.044
#&gt; GSM38729     1   0.000      0.993 1.000 0.000
#&gt; GSM38730     1   0.000      0.993 1.000 0.000
#&gt; GSM38731     1   0.000      0.993 1.000 0.000
#&gt; GSM38732     2   0.311      0.930 0.056 0.944
#&gt; GSM38733     1   0.000      0.993 1.000 0.000
#&gt; GSM38734     2   0.000      0.979 0.000 1.000
#&gt; GSM38735     1   0.000      0.993 1.000 0.000
#&gt; GSM38736     2   0.000      0.979 0.000 1.000
#&gt; GSM38737     2   0.000      0.979 0.000 1.000
#&gt; GSM38738     2   0.775      0.707 0.228 0.772
#&gt; GSM38739     1   0.000      0.993 1.000 0.000
#&gt; GSM38740     1   0.000      0.993 1.000 0.000
#&gt; GSM38741     1   0.311      0.946 0.944 0.056
#&gt; GSM38742     2   0.000      0.979 0.000 1.000
#&gt; GSM38743     2   0.000      0.979 0.000 1.000
#&gt; GSM38744     1   0.000      0.993 1.000 0.000
#&gt; GSM38745     1   0.000      0.993 1.000 0.000
#&gt; GSM38746     1   0.260      0.958 0.956 0.044
#&gt; GSM38747     1   0.260      0.958 0.956 0.044
#&gt; GSM38748     2   0.000      0.979 0.000 1.000
#&gt; GSM38749     1   0.000      0.993 1.000 0.000
#&gt; GSM38750     1   0.000      0.993 1.000 0.000
#&gt; GSM38751     1   0.000      0.993 1.000 0.000
#&gt; GSM38752     2   0.000      0.979 0.000 1.000
#&gt; GSM38753     2   0.000      0.979 0.000 1.000
#&gt; GSM38754     2   0.000      0.979 0.000 1.000
#&gt; GSM38755     1   0.000      0.993 1.000 0.000
#&gt; GSM38756     2   0.000      0.979 0.000 1.000
#&gt; GSM38757     1   0.000      0.993 1.000 0.000
#&gt; GSM38758     2   0.000      0.979 0.000 1.000
#&gt; GSM38759     1   0.260      0.958 0.956 0.044
#&gt; GSM38760     1   0.000      0.993 1.000 0.000
#&gt; GSM38761     2   0.000      0.979 0.000 1.000
#&gt; GSM38762     2   0.000      0.979 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.2066      0.881 0.940 0.000 0.060
#&gt; GSM38713     1  0.3879      0.808 0.848 0.000 0.152
#&gt; GSM38714     1  0.3879      0.808 0.848 0.000 0.152
#&gt; GSM38715     1  0.3879      0.808 0.848 0.000 0.152
#&gt; GSM38716     1  0.2066      0.881 0.940 0.000 0.060
#&gt; GSM38717     1  0.2066      0.881 0.940 0.000 0.060
#&gt; GSM38718     1  0.2066      0.881 0.940 0.000 0.060
#&gt; GSM38719     1  0.2066      0.881 0.940 0.000 0.060
#&gt; GSM38720     1  0.2066      0.881 0.940 0.000 0.060
#&gt; GSM38721     1  0.6079      0.305 0.612 0.000 0.388
#&gt; GSM38722     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38724     3  0.4974      0.780 0.236 0.000 0.764
#&gt; GSM38725     1  0.0237      0.891 0.996 0.000 0.004
#&gt; GSM38726     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38728     3  0.1411      0.807 0.036 0.000 0.964
#&gt; GSM38729     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38732     2  0.3038      0.907 0.000 0.896 0.104
#&gt; GSM38733     1  0.6079      0.305 0.612 0.000 0.388
#&gt; GSM38734     2  0.1753      0.946 0.000 0.952 0.048
#&gt; GSM38735     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38736     2  0.0892      0.959 0.000 0.980 0.020
#&gt; GSM38737     2  0.0892      0.959 0.000 0.980 0.020
#&gt; GSM38738     2  0.5698      0.689 0.012 0.736 0.252
#&gt; GSM38739     1  0.4605      0.692 0.796 0.000 0.204
#&gt; GSM38740     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38741     3  0.1877      0.803 0.032 0.012 0.956
#&gt; GSM38742     2  0.0892      0.959 0.000 0.980 0.020
#&gt; GSM38743     2  0.0892      0.959 0.000 0.980 0.020
#&gt; GSM38744     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38745     1  0.0000      0.893 1.000 0.000 0.000
#&gt; GSM38746     3  0.1289      0.806 0.032 0.000 0.968
#&gt; GSM38747     3  0.1289      0.806 0.032 0.000 0.968
#&gt; GSM38748     2  0.0592      0.957 0.000 0.988 0.012
#&gt; GSM38749     1  0.4605      0.692 0.796 0.000 0.204
#&gt; GSM38750     3  0.4887      0.787 0.228 0.000 0.772
#&gt; GSM38751     3  0.4887      0.787 0.228 0.000 0.772
#&gt; GSM38752     2  0.1753      0.946 0.000 0.952 0.048
#&gt; GSM38753     2  0.0592      0.957 0.000 0.988 0.012
#&gt; GSM38754     2  0.1753      0.946 0.000 0.952 0.048
#&gt; GSM38755     3  0.5706      0.658 0.320 0.000 0.680
#&gt; GSM38756     2  0.0592      0.957 0.000 0.988 0.012
#&gt; GSM38757     3  0.4842      0.789 0.224 0.000 0.776
#&gt; GSM38758     2  0.0892      0.959 0.000 0.980 0.020
#&gt; GSM38759     3  0.1529      0.807 0.040 0.000 0.960
#&gt; GSM38760     3  0.6299      0.232 0.476 0.000 0.524
#&gt; GSM38761     2  0.0892      0.959 0.000 0.980 0.020
#&gt; GSM38762     2  0.0892      0.959 0.000 0.980 0.020
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.1792      0.873 0.932 0.000 0.068 0.000
#&gt; GSM38713     1  0.3172      0.804 0.840 0.000 0.160 0.000
#&gt; GSM38714     1  0.3172      0.804 0.840 0.000 0.160 0.000
#&gt; GSM38715     1  0.3172      0.804 0.840 0.000 0.160 0.000
#&gt; GSM38716     1  0.1792      0.873 0.932 0.000 0.068 0.000
#&gt; GSM38717     1  0.1792      0.873 0.932 0.000 0.068 0.000
#&gt; GSM38718     1  0.1792      0.873 0.932 0.000 0.068 0.000
#&gt; GSM38719     1  0.1792      0.873 0.932 0.000 0.068 0.000
#&gt; GSM38720     1  0.1792      0.873 0.932 0.000 0.068 0.000
#&gt; GSM38721     1  0.4907      0.256 0.580 0.000 0.420 0.000
#&gt; GSM38722     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.3444      0.797 0.184 0.000 0.816 0.000
#&gt; GSM38725     1  0.0188      0.883 0.996 0.000 0.004 0.000
#&gt; GSM38726     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM38728     3  0.0992      0.774 0.004 0.012 0.976 0.008
#&gt; GSM38729     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM38732     4  0.1716      0.878 0.000 0.000 0.064 0.936
#&gt; GSM38733     1  0.4907      0.256 0.580 0.000 0.420 0.000
#&gt; GSM38734     4  0.0336      0.897 0.000 0.000 0.008 0.992
#&gt; GSM38735     1  0.1229      0.874 0.968 0.004 0.020 0.008
#&gt; GSM38736     2  0.0592      1.000 0.000 0.984 0.000 0.016
#&gt; GSM38737     2  0.0592      1.000 0.000 0.984 0.000 0.016
#&gt; GSM38738     4  0.3942      0.697 0.000 0.000 0.236 0.764
#&gt; GSM38739     1  0.3837      0.683 0.776 0.000 0.224 0.000
#&gt; GSM38740     1  0.1229      0.874 0.968 0.004 0.020 0.008
#&gt; GSM38741     3  0.0895      0.767 0.000 0.004 0.976 0.020
#&gt; GSM38742     2  0.0592      1.000 0.000 0.984 0.000 0.016
#&gt; GSM38743     2  0.0592      1.000 0.000 0.984 0.000 0.016
#&gt; GSM38744     1  0.1229      0.874 0.968 0.004 0.020 0.008
#&gt; GSM38745     1  0.1229      0.874 0.968 0.004 0.020 0.008
#&gt; GSM38746     3  0.0804      0.772 0.000 0.012 0.980 0.008
#&gt; GSM38747     3  0.0804      0.772 0.000 0.012 0.980 0.008
#&gt; GSM38748     4  0.1118      0.889 0.000 0.036 0.000 0.964
#&gt; GSM38749     1  0.3837      0.683 0.776 0.000 0.224 0.000
#&gt; GSM38750     3  0.3356      0.804 0.176 0.000 0.824 0.000
#&gt; GSM38751     3  0.3356      0.804 0.176 0.000 0.824 0.000
#&gt; GSM38752     4  0.0336      0.897 0.000 0.000 0.008 0.992
#&gt; GSM38753     4  0.3266      0.808 0.000 0.168 0.000 0.832
#&gt; GSM38754     4  0.0336      0.897 0.000 0.000 0.008 0.992
#&gt; GSM38755     3  0.4222      0.678 0.272 0.000 0.728 0.000
#&gt; GSM38756     4  0.3266      0.808 0.000 0.168 0.000 0.832
#&gt; GSM38757     3  0.3311      0.805 0.172 0.000 0.828 0.000
#&gt; GSM38758     2  0.0592      1.000 0.000 0.984 0.000 0.016
#&gt; GSM38759     3  0.1139      0.775 0.008 0.012 0.972 0.008
#&gt; GSM38760     3  0.4948      0.242 0.440 0.000 0.560 0.000
#&gt; GSM38761     2  0.0592      1.000 0.000 0.984 0.000 0.016
#&gt; GSM38762     2  0.0592      1.000 0.000 0.984 0.000 0.016
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0000      0.822 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.2260      0.785 0.908 0.000 0.028 0.000 0.064
#&gt; GSM38714     1  0.2260      0.785 0.908 0.000 0.028 0.000 0.064
#&gt; GSM38715     1  0.2260      0.785 0.908 0.000 0.028 0.000 0.064
#&gt; GSM38716     1  0.0000      0.822 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.822 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.822 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.822 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.822 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.5067      0.418 0.648 0.000 0.288 0.000 0.064
#&gt; GSM38722     1  0.2966      0.752 0.816 0.000 0.000 0.000 0.184
#&gt; GSM38723     1  0.3336      0.714 0.772 0.000 0.000 0.000 0.228
#&gt; GSM38724     3  0.5301      0.744 0.176 0.000 0.676 0.000 0.148
#&gt; GSM38725     1  0.2233      0.806 0.892 0.000 0.004 0.000 0.104
#&gt; GSM38726     1  0.2020      0.808 0.900 0.000 0.000 0.000 0.100
#&gt; GSM38727     1  0.3336      0.714 0.772 0.000 0.000 0.000 0.228
#&gt; GSM38728     3  0.3055      0.750 0.072 0.000 0.864 0.000 0.064
#&gt; GSM38729     1  0.2020      0.808 0.900 0.000 0.000 0.000 0.100
#&gt; GSM38730     1  0.2020      0.808 0.900 0.000 0.000 0.000 0.100
#&gt; GSM38731     1  0.2020      0.808 0.900 0.000 0.000 0.000 0.100
#&gt; GSM38732     4  0.1341      0.864 0.000 0.000 0.056 0.944 0.000
#&gt; GSM38733     1  0.5067      0.418 0.648 0.000 0.288 0.000 0.064
#&gt; GSM38734     4  0.0000      0.882 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38735     5  0.1732      1.000 0.080 0.000 0.000 0.000 0.920
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38738     4  0.3461      0.676 0.000 0.000 0.224 0.772 0.004
#&gt; GSM38739     1  0.6366      0.410 0.512 0.000 0.204 0.000 0.284
#&gt; GSM38740     5  0.1732      1.000 0.080 0.000 0.000 0.000 0.920
#&gt; GSM38741     3  0.0404      0.764 0.000 0.000 0.988 0.012 0.000
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.1732      1.000 0.080 0.000 0.000 0.000 0.920
#&gt; GSM38745     5  0.1732      1.000 0.080 0.000 0.000 0.000 0.920
#&gt; GSM38746     3  0.0162      0.765 0.000 0.000 0.996 0.000 0.004
#&gt; GSM38747     3  0.0162      0.765 0.000 0.000 0.996 0.000 0.004
#&gt; GSM38748     4  0.1124      0.880 0.000 0.036 0.000 0.960 0.004
#&gt; GSM38749     1  0.6366      0.410 0.512 0.000 0.204 0.000 0.284
#&gt; GSM38750     3  0.3918      0.783 0.096 0.000 0.804 0.000 0.100
#&gt; GSM38751     3  0.3918      0.783 0.096 0.000 0.804 0.000 0.100
#&gt; GSM38752     4  0.0404      0.884 0.000 0.000 0.000 0.988 0.012
#&gt; GSM38753     4  0.3456      0.789 0.000 0.184 0.000 0.800 0.016
#&gt; GSM38754     4  0.0404      0.884 0.000 0.000 0.000 0.988 0.012
#&gt; GSM38755     3  0.5358      0.683 0.248 0.000 0.648 0.000 0.104
#&gt; GSM38756     4  0.3456      0.789 0.000 0.184 0.000 0.800 0.016
#&gt; GSM38757     3  0.3918      0.785 0.096 0.000 0.804 0.000 0.100
#&gt; GSM38758     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38759     3  0.3116      0.750 0.076 0.000 0.860 0.000 0.064
#&gt; GSM38760     3  0.6344      0.426 0.344 0.000 0.484 0.000 0.172
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.0000      0.810 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.2123      0.774 0.908 0.000 0.008 0.000 0.064 0.020
#&gt; GSM38714     1  0.2123      0.774 0.908 0.000 0.008 0.000 0.064 0.020
#&gt; GSM38715     1  0.2123      0.774 0.908 0.000 0.008 0.000 0.064 0.020
#&gt; GSM38716     1  0.0000      0.810 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.810 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.810 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.810 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.810 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.4957      0.476 0.648 0.000 0.268 0.000 0.064 0.020
#&gt; GSM38722     1  0.3279      0.738 0.796 0.000 0.000 0.000 0.176 0.028
#&gt; GSM38723     1  0.3727      0.698 0.748 0.000 0.000 0.000 0.216 0.036
#&gt; GSM38724     3  0.5754      0.730 0.080 0.000 0.596 0.000 0.060 0.264
#&gt; GSM38725     1  0.2537      0.792 0.872 0.000 0.000 0.000 0.096 0.032
#&gt; GSM38726     1  0.2412      0.794 0.880 0.000 0.000 0.000 0.092 0.028
#&gt; GSM38727     1  0.3727      0.698 0.748 0.000 0.000 0.000 0.216 0.036
#&gt; GSM38728     3  0.3275      0.693 0.072 0.000 0.844 0.000 0.064 0.020
#&gt; GSM38729     1  0.2412      0.794 0.880 0.000 0.000 0.000 0.092 0.028
#&gt; GSM38730     1  0.2412      0.794 0.880 0.000 0.000 0.000 0.092 0.028
#&gt; GSM38731     1  0.2412      0.794 0.880 0.000 0.000 0.000 0.092 0.028
#&gt; GSM38732     4  0.4020      0.658 0.000 0.000 0.032 0.692 0.000 0.276
#&gt; GSM38733     1  0.4957      0.476 0.648 0.000 0.268 0.000 0.064 0.020
#&gt; GSM38734     4  0.3151      0.657 0.000 0.000 0.000 0.748 0.000 0.252
#&gt; GSM38735     5  0.1327      1.000 0.064 0.000 0.000 0.000 0.936 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38738     4  0.5684      0.477 0.000 0.000 0.200 0.520 0.000 0.280
#&gt; GSM38739     1  0.7360      0.272 0.416 0.000 0.188 0.000 0.216 0.180
#&gt; GSM38740     5  0.1327      1.000 0.064 0.000 0.000 0.000 0.936 0.000
#&gt; GSM38741     3  0.0508      0.742 0.000 0.000 0.984 0.012 0.000 0.004
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.1327      1.000 0.064 0.000 0.000 0.000 0.936 0.000
#&gt; GSM38745     5  0.1327      1.000 0.064 0.000 0.000 0.000 0.936 0.000
#&gt; GSM38746     3  0.0000      0.743 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38747     3  0.0000      0.743 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38748     6  0.4344      0.275 0.000 0.036 0.000 0.336 0.000 0.628
#&gt; GSM38749     1  0.7360      0.272 0.416 0.000 0.188 0.000 0.216 0.180
#&gt; GSM38750     3  0.3266      0.756 0.000 0.000 0.728 0.000 0.000 0.272
#&gt; GSM38751     3  0.3266      0.756 0.000 0.000 0.728 0.000 0.000 0.272
#&gt; GSM38752     4  0.0000      0.560 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38753     6  0.5820      0.744 0.000 0.184 0.000 0.400 0.000 0.416
#&gt; GSM38754     4  0.0000      0.560 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38755     3  0.5351      0.688 0.148 0.000 0.572 0.000 0.000 0.280
#&gt; GSM38756     6  0.5820      0.744 0.000 0.184 0.000 0.400 0.000 0.416
#&gt; GSM38757     3  0.3221      0.759 0.000 0.000 0.736 0.000 0.000 0.264
#&gt; GSM38758     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38759     3  0.3329      0.691 0.076 0.000 0.840 0.000 0.064 0.020
#&gt; GSM38760     3  0.6871      0.486 0.220 0.000 0.408 0.000 0.060 0.312
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n individual(p) k
#> MAD:hclust 51      0.136757 2
#> MAD:hclust 48      0.033987 3
#> MAD:hclust 48      0.004140 4
#> MAD:hclust 46      0.000429 5
#> MAD:hclust 44      0.001062 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.849           0.933       0.967         0.4293 0.594   0.594
#> 3 3 0.676           0.780       0.882         0.4509 0.757   0.596
#> 4 4 0.629           0.621       0.777         0.1514 0.817   0.553
#> 5 5 0.768           0.813       0.852         0.0727 0.964   0.869
#> 6 6 0.773           0.763       0.839         0.0568 0.917   0.677
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.952 1.000 0.000
#&gt; GSM38713     1  0.0000      0.952 1.000 0.000
#&gt; GSM38714     1  0.0000      0.952 1.000 0.000
#&gt; GSM38715     1  0.0000      0.952 1.000 0.000
#&gt; GSM38716     1  0.0000      0.952 1.000 0.000
#&gt; GSM38717     1  0.0000      0.952 1.000 0.000
#&gt; GSM38718     1  0.0000      0.952 1.000 0.000
#&gt; GSM38719     1  0.0000      0.952 1.000 0.000
#&gt; GSM38720     1  0.0000      0.952 1.000 0.000
#&gt; GSM38721     1  0.0000      0.952 1.000 0.000
#&gt; GSM38722     1  0.0000      0.952 1.000 0.000
#&gt; GSM38723     1  0.0000      0.952 1.000 0.000
#&gt; GSM38724     1  0.1184      0.944 0.984 0.016
#&gt; GSM38725     1  0.0000      0.952 1.000 0.000
#&gt; GSM38726     1  0.0000      0.952 1.000 0.000
#&gt; GSM38727     1  0.0000      0.952 1.000 0.000
#&gt; GSM38728     1  0.4815      0.880 0.896 0.104
#&gt; GSM38729     1  0.0000      0.952 1.000 0.000
#&gt; GSM38730     1  0.0000      0.952 1.000 0.000
#&gt; GSM38731     1  0.0000      0.952 1.000 0.000
#&gt; GSM38732     1  0.9608      0.468 0.616 0.384
#&gt; GSM38733     1  0.0000      0.952 1.000 0.000
#&gt; GSM38734     2  0.0000      1.000 0.000 1.000
#&gt; GSM38735     1  0.0672      0.948 0.992 0.008
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000
#&gt; GSM38738     1  0.9323      0.546 0.652 0.348
#&gt; GSM38739     1  0.0000      0.952 1.000 0.000
#&gt; GSM38740     1  0.0000      0.952 1.000 0.000
#&gt; GSM38741     2  0.0000      1.000 0.000 1.000
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000
#&gt; GSM38744     1  0.0000      0.952 1.000 0.000
#&gt; GSM38745     1  0.0376      0.950 0.996 0.004
#&gt; GSM38746     1  0.4562      0.887 0.904 0.096
#&gt; GSM38747     1  0.4562      0.887 0.904 0.096
#&gt; GSM38748     2  0.0000      1.000 0.000 1.000
#&gt; GSM38749     1  0.0000      0.952 1.000 0.000
#&gt; GSM38750     1  0.7528      0.762 0.784 0.216
#&gt; GSM38751     1  0.7528      0.762 0.784 0.216
#&gt; GSM38752     2  0.0000      1.000 0.000 1.000
#&gt; GSM38753     2  0.0000      1.000 0.000 1.000
#&gt; GSM38754     2  0.0000      1.000 0.000 1.000
#&gt; GSM38755     1  0.0000      0.952 1.000 0.000
#&gt; GSM38756     2  0.0000      1.000 0.000 1.000
#&gt; GSM38757     1  0.7299      0.777 0.796 0.204
#&gt; GSM38758     2  0.0000      1.000 0.000 1.000
#&gt; GSM38759     1  0.1184      0.944 0.984 0.016
#&gt; GSM38760     1  0.0000      0.952 1.000 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38713     1  0.1289      0.884 0.968 0.000 0.032
#&gt; GSM38714     1  0.1289      0.884 0.968 0.000 0.032
#&gt; GSM38715     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38721     1  0.1289      0.884 0.968 0.000 0.032
#&gt; GSM38722     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38724     3  0.5560      0.651 0.300 0.000 0.700
#&gt; GSM38725     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38728     3  0.1753      0.810 0.048 0.000 0.952
#&gt; GSM38729     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.902 1.000 0.000 0.000
#&gt; GSM38732     3  0.1753      0.810 0.048 0.000 0.952
#&gt; GSM38733     1  0.3686      0.782 0.860 0.000 0.140
#&gt; GSM38734     2  0.6215      0.608 0.000 0.572 0.428
#&gt; GSM38735     1  0.6319      0.676 0.732 0.040 0.228
#&gt; GSM38736     2  0.0424      0.815 0.000 0.992 0.008
#&gt; GSM38737     2  0.0424      0.815 0.000 0.992 0.008
#&gt; GSM38738     3  0.1753      0.810 0.048 0.000 0.952
#&gt; GSM38739     1  0.6079      0.308 0.612 0.000 0.388
#&gt; GSM38740     1  0.5292      0.709 0.764 0.008 0.228
#&gt; GSM38741     3  0.1753      0.730 0.000 0.048 0.952
#&gt; GSM38742     2  0.0000      0.816 0.000 1.000 0.000
#&gt; GSM38743     2  0.0424      0.815 0.000 0.992 0.008
#&gt; GSM38744     1  0.1753      0.864 0.952 0.000 0.048
#&gt; GSM38745     1  0.6319      0.676 0.732 0.040 0.228
#&gt; GSM38746     3  0.5560      0.651 0.300 0.000 0.700
#&gt; GSM38747     3  0.5560      0.651 0.300 0.000 0.700
#&gt; GSM38748     2  0.5678      0.721 0.000 0.684 0.316
#&gt; GSM38749     1  0.5810      0.446 0.664 0.000 0.336
#&gt; GSM38750     3  0.1753      0.810 0.048 0.000 0.952
#&gt; GSM38751     3  0.1753      0.810 0.048 0.000 0.952
#&gt; GSM38752     2  0.6192      0.620 0.000 0.580 0.420
#&gt; GSM38753     2  0.5363      0.747 0.000 0.724 0.276
#&gt; GSM38754     2  0.6192      0.620 0.000 0.580 0.420
#&gt; GSM38755     3  0.3551      0.776 0.132 0.000 0.868
#&gt; GSM38756     2  0.5363      0.747 0.000 0.724 0.276
#&gt; GSM38757     3  0.1753      0.810 0.048 0.000 0.952
#&gt; GSM38758     2  0.0000      0.816 0.000 1.000 0.000
#&gt; GSM38759     3  0.5968      0.506 0.364 0.000 0.636
#&gt; GSM38760     1  0.5560      0.531 0.700 0.000 0.300
#&gt; GSM38761     2  0.0000      0.816 0.000 1.000 0.000
#&gt; GSM38762     2  0.0424      0.815 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.937 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.2300      0.911 0.924 0.048 0.028 0.000
#&gt; GSM38714     1  0.2300      0.911 0.924 0.048 0.028 0.000
#&gt; GSM38715     1  0.1975      0.919 0.936 0.048 0.016 0.000
#&gt; GSM38716     1  0.0000      0.937 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0817      0.933 0.976 0.024 0.000 0.000
#&gt; GSM38718     1  0.1576      0.924 0.948 0.048 0.004 0.000
#&gt; GSM38719     1  0.0817      0.933 0.976 0.024 0.000 0.000
#&gt; GSM38720     1  0.0000      0.937 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.2300      0.911 0.924 0.048 0.028 0.000
#&gt; GSM38722     1  0.2760      0.830 0.872 0.128 0.000 0.000
#&gt; GSM38723     1  0.3123      0.798 0.844 0.156 0.000 0.000
#&gt; GSM38724     3  0.2216      0.746 0.092 0.000 0.908 0.000
#&gt; GSM38725     1  0.0000      0.937 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.937 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.3123      0.798 0.844 0.156 0.000 0.000
#&gt; GSM38728     3  0.3505      0.715 0.000 0.048 0.864 0.088
#&gt; GSM38729     1  0.0000      0.937 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.937 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.937 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.4004      0.664 0.000 0.024 0.812 0.164
#&gt; GSM38733     1  0.3758      0.825 0.848 0.048 0.104 0.000
#&gt; GSM38734     4  0.3688      0.635 0.000 0.000 0.208 0.792
#&gt; GSM38735     2  0.6924      0.336 0.232 0.588 0.180 0.000
#&gt; GSM38736     2  0.5000     -0.174 0.000 0.504 0.000 0.496
#&gt; GSM38737     2  0.5000     -0.174 0.000 0.504 0.000 0.496
#&gt; GSM38738     3  0.3219      0.675 0.000 0.000 0.836 0.164
#&gt; GSM38739     3  0.7271      0.378 0.244 0.216 0.540 0.000
#&gt; GSM38740     2  0.7095      0.320 0.260 0.560 0.180 0.000
#&gt; GSM38741     3  0.3311      0.666 0.000 0.000 0.828 0.172
#&gt; GSM38742     4  0.4948      0.145 0.000 0.440 0.000 0.560
#&gt; GSM38743     2  0.5000     -0.174 0.000 0.504 0.000 0.496
#&gt; GSM38744     2  0.7264      0.273 0.320 0.512 0.168 0.000
#&gt; GSM38745     2  0.6924      0.336 0.232 0.588 0.180 0.000
#&gt; GSM38746     3  0.3004      0.734 0.060 0.048 0.892 0.000
#&gt; GSM38747     3  0.1970      0.750 0.060 0.008 0.932 0.000
#&gt; GSM38748     4  0.2647      0.658 0.000 0.000 0.120 0.880
#&gt; GSM38749     3  0.7483      0.324 0.288 0.216 0.496 0.000
#&gt; GSM38750     3  0.2197      0.751 0.000 0.024 0.928 0.048
#&gt; GSM38751     3  0.1610      0.752 0.000 0.032 0.952 0.016
#&gt; GSM38752     4  0.3649      0.639 0.000 0.000 0.204 0.796
#&gt; GSM38753     4  0.1637      0.650 0.000 0.000 0.060 0.940
#&gt; GSM38754     4  0.3649      0.639 0.000 0.000 0.204 0.796
#&gt; GSM38755     3  0.2739      0.752 0.000 0.060 0.904 0.036
#&gt; GSM38756     4  0.1637      0.650 0.000 0.000 0.060 0.940
#&gt; GSM38757     3  0.1389      0.747 0.000 0.000 0.952 0.048
#&gt; GSM38758     4  0.4817      0.231 0.000 0.388 0.000 0.612
#&gt; GSM38759     3  0.4307      0.687 0.144 0.048 0.808 0.000
#&gt; GSM38760     3  0.7665      0.195 0.360 0.216 0.424 0.000
#&gt; GSM38761     4  0.4948      0.145 0.000 0.440 0.000 0.560
#&gt; GSM38762     2  0.5000     -0.174 0.000 0.504 0.000 0.496
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0609      0.842 0.980 0.000 0.000 0.000 0.020
#&gt; GSM38713     1  0.4832      0.753 0.748 0.000 0.016 0.152 0.084
#&gt; GSM38714     1  0.4832      0.753 0.748 0.000 0.016 0.152 0.084
#&gt; GSM38715     1  0.4733      0.756 0.752 0.000 0.012 0.152 0.084
#&gt; GSM38716     1  0.0992      0.841 0.968 0.000 0.000 0.008 0.024
#&gt; GSM38717     1  0.1403      0.838 0.952 0.000 0.000 0.024 0.024
#&gt; GSM38718     1  0.3921      0.780 0.800 0.000 0.000 0.128 0.072
#&gt; GSM38719     1  0.1310      0.838 0.956 0.000 0.000 0.020 0.024
#&gt; GSM38720     1  0.0609      0.841 0.980 0.000 0.000 0.020 0.000
#&gt; GSM38721     1  0.4832      0.753 0.748 0.000 0.016 0.152 0.084
#&gt; GSM38722     1  0.3445      0.721 0.824 0.000 0.000 0.036 0.140
#&gt; GSM38723     1  0.4087      0.626 0.756 0.000 0.000 0.036 0.208
#&gt; GSM38724     3  0.1399      0.818 0.000 0.000 0.952 0.028 0.020
#&gt; GSM38725     1  0.1195      0.840 0.960 0.000 0.000 0.012 0.028
#&gt; GSM38726     1  0.1310      0.837 0.956 0.000 0.000 0.020 0.024
#&gt; GSM38727     1  0.4087      0.626 0.756 0.000 0.000 0.036 0.208
#&gt; GSM38728     3  0.5108      0.676 0.020 0.000 0.728 0.160 0.092
#&gt; GSM38729     1  0.0609      0.842 0.980 0.000 0.000 0.000 0.020
#&gt; GSM38730     1  0.1310      0.837 0.956 0.000 0.000 0.020 0.024
#&gt; GSM38731     1  0.1310      0.837 0.956 0.000 0.000 0.020 0.024
#&gt; GSM38732     3  0.2522      0.801 0.000 0.000 0.880 0.108 0.012
#&gt; GSM38733     1  0.4979      0.747 0.740 0.000 0.020 0.152 0.088
#&gt; GSM38734     4  0.4062      0.878 0.000 0.132 0.068 0.796 0.004
#&gt; GSM38735     5  0.3423      0.984 0.080 0.044 0.020 0.000 0.856
#&gt; GSM38736     2  0.0703      0.961 0.000 0.976 0.000 0.000 0.024
#&gt; GSM38737     2  0.0703      0.961 0.000 0.976 0.000 0.000 0.024
#&gt; GSM38738     3  0.2193      0.808 0.000 0.000 0.900 0.092 0.008
#&gt; GSM38739     3  0.5940      0.503 0.092 0.000 0.640 0.032 0.236
#&gt; GSM38740     5  0.3815      0.982 0.080 0.044 0.020 0.012 0.844
#&gt; GSM38741     3  0.2248      0.805 0.000 0.000 0.900 0.088 0.012
#&gt; GSM38742     2  0.0898      0.947 0.000 0.972 0.000 0.020 0.008
#&gt; GSM38743     2  0.0703      0.961 0.000 0.976 0.000 0.000 0.024
#&gt; GSM38744     5  0.3654      0.961 0.104 0.020 0.020 0.012 0.844
#&gt; GSM38745     5  0.3423      0.984 0.080 0.044 0.020 0.000 0.856
#&gt; GSM38746     3  0.0794      0.823 0.000 0.000 0.972 0.000 0.028
#&gt; GSM38747     3  0.0510      0.824 0.000 0.000 0.984 0.000 0.016
#&gt; GSM38748     4  0.4414      0.884 0.000 0.248 0.008 0.720 0.024
#&gt; GSM38749     3  0.6415      0.447 0.132 0.000 0.600 0.036 0.232
#&gt; GSM38750     3  0.1117      0.822 0.000 0.000 0.964 0.016 0.020
#&gt; GSM38751     3  0.0798      0.823 0.000 0.000 0.976 0.008 0.016
#&gt; GSM38752     4  0.4017      0.892 0.000 0.148 0.064 0.788 0.000
#&gt; GSM38753     4  0.4106      0.879 0.000 0.256 0.000 0.724 0.020
#&gt; GSM38754     4  0.4017      0.892 0.000 0.148 0.064 0.788 0.000
#&gt; GSM38755     3  0.1830      0.814 0.000 0.000 0.932 0.028 0.040
#&gt; GSM38756     4  0.4106      0.879 0.000 0.256 0.000 0.724 0.020
#&gt; GSM38757     3  0.0671      0.824 0.000 0.000 0.980 0.016 0.004
#&gt; GSM38758     2  0.1557      0.912 0.000 0.940 0.000 0.052 0.008
#&gt; GSM38759     3  0.5846      0.618 0.044 0.000 0.672 0.192 0.092
#&gt; GSM38760     3  0.7022      0.293 0.208 0.000 0.516 0.036 0.240
#&gt; GSM38761     2  0.0898      0.947 0.000 0.972 0.000 0.020 0.008
#&gt; GSM38762     2  0.0703      0.961 0.000 0.976 0.000 0.000 0.024
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1152     0.8462 0.952 0.000 0.000 0.004 0.000 0.044
#&gt; GSM38713     6  0.3830     0.6615 0.376 0.000 0.000 0.004 0.000 0.620
#&gt; GSM38714     6  0.3830     0.6615 0.376 0.000 0.000 0.004 0.000 0.620
#&gt; GSM38715     6  0.3852     0.6479 0.384 0.000 0.000 0.004 0.000 0.612
#&gt; GSM38716     1  0.1010     0.8490 0.960 0.000 0.000 0.004 0.000 0.036
#&gt; GSM38717     1  0.2053     0.7945 0.888 0.000 0.000 0.004 0.000 0.108
#&gt; GSM38718     1  0.3982    -0.2903 0.536 0.000 0.000 0.004 0.000 0.460
#&gt; GSM38719     1  0.2006     0.7991 0.892 0.000 0.000 0.004 0.000 0.104
#&gt; GSM38720     1  0.1908     0.8061 0.900 0.000 0.000 0.004 0.000 0.096
#&gt; GSM38721     6  0.3695     0.6623 0.376 0.000 0.000 0.000 0.000 0.624
#&gt; GSM38722     1  0.2164     0.7759 0.900 0.000 0.000 0.000 0.032 0.068
#&gt; GSM38723     1  0.3210     0.7346 0.852 0.000 0.000 0.032 0.048 0.068
#&gt; GSM38724     3  0.4124     0.6229 0.000 0.008 0.648 0.012 0.000 0.332
#&gt; GSM38725     1  0.0717     0.8485 0.976 0.008 0.000 0.000 0.000 0.016
#&gt; GSM38726     1  0.0000     0.8500 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.3210     0.7346 0.852 0.000 0.000 0.032 0.048 0.068
#&gt; GSM38728     6  0.4786     0.0222 0.000 0.008 0.256 0.076 0.000 0.660
#&gt; GSM38729     1  0.1007     0.8475 0.956 0.000 0.000 0.000 0.000 0.044
#&gt; GSM38730     1  0.0000     0.8500 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000     0.8500 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.4780     0.7015 0.000 0.012 0.688 0.092 0.000 0.208
#&gt; GSM38733     6  0.3428     0.6591 0.304 0.000 0.000 0.000 0.000 0.696
#&gt; GSM38734     4  0.2585     0.8959 0.000 0.068 0.004 0.880 0.000 0.048
#&gt; GSM38735     5  0.1096     0.9887 0.008 0.004 0.000 0.004 0.964 0.020
#&gt; GSM38736     2  0.1075     0.9562 0.000 0.952 0.000 0.000 0.048 0.000
#&gt; GSM38737     2  0.1075     0.9562 0.000 0.952 0.000 0.000 0.048 0.000
#&gt; GSM38738     3  0.4467     0.7224 0.000 0.012 0.724 0.080 0.000 0.184
#&gt; GSM38739     3  0.5920     0.5963 0.112 0.012 0.692 0.040 0.072 0.072
#&gt; GSM38740     5  0.0405     0.9877 0.008 0.004 0.000 0.000 0.988 0.000
#&gt; GSM38741     3  0.4815     0.6967 0.000 0.008 0.668 0.088 0.000 0.236
#&gt; GSM38742     2  0.1334     0.9404 0.000 0.948 0.000 0.032 0.000 0.020
#&gt; GSM38743     2  0.1075     0.9562 0.000 0.952 0.000 0.000 0.048 0.000
#&gt; GSM38744     5  0.0260     0.9864 0.008 0.000 0.000 0.000 0.992 0.000
#&gt; GSM38745     5  0.1096     0.9887 0.008 0.004 0.000 0.004 0.964 0.020
#&gt; GSM38746     3  0.1964     0.7655 0.000 0.004 0.920 0.008 0.012 0.056
#&gt; GSM38747     3  0.2326     0.7634 0.000 0.008 0.888 0.012 0.000 0.092
#&gt; GSM38748     4  0.2431     0.8990 0.000 0.132 0.000 0.860 0.008 0.000
#&gt; GSM38749     3  0.6183     0.5700 0.140 0.012 0.664 0.040 0.072 0.072
#&gt; GSM38750     3  0.0508     0.7680 0.000 0.000 0.984 0.004 0.012 0.000
#&gt; GSM38751     3  0.0363     0.7684 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM38752     4  0.3038     0.8952 0.000 0.060 0.012 0.856 0.000 0.072
#&gt; GSM38753     4  0.2593     0.8935 0.000 0.148 0.000 0.844 0.008 0.000
#&gt; GSM38754     4  0.3038     0.8952 0.000 0.060 0.012 0.856 0.000 0.072
#&gt; GSM38755     3  0.3610     0.7687 0.000 0.020 0.812 0.008 0.024 0.136
#&gt; GSM38756     4  0.2593     0.8935 0.000 0.148 0.000 0.844 0.008 0.000
#&gt; GSM38757     3  0.2462     0.7668 0.000 0.004 0.860 0.004 0.000 0.132
#&gt; GSM38758     2  0.1807     0.9198 0.000 0.920 0.000 0.060 0.000 0.020
#&gt; GSM38759     6  0.3707     0.3419 0.020 0.008 0.164 0.016 0.000 0.792
#&gt; GSM38760     3  0.6674     0.4989 0.208 0.012 0.596 0.040 0.072 0.072
#&gt; GSM38761     2  0.1334     0.9404 0.000 0.948 0.000 0.032 0.000 0.020
#&gt; GSM38762     2  0.1333     0.9533 0.000 0.944 0.000 0.000 0.048 0.008
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n individual(p) k
#> MAD:kmeans 50      0.055542 2
#> MAD:kmeans 49      0.032939 3
#> MAD:kmeans 37      0.034876 4
#> MAD:kmeans 49      0.000212 5
#> MAD:kmeans 47      0.000536 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.918           0.924       0.967         0.5004 0.506   0.506
#> 3 3 0.943           0.950       0.975         0.3048 0.733   0.523
#> 4 4 0.794           0.822       0.860         0.1237 0.882   0.675
#> 5 5 0.735           0.715       0.831         0.0579 0.960   0.850
#> 6 6 0.767           0.732       0.849         0.0553 0.896   0.593
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.948 1.000 0.000
#&gt; GSM38713     1   0.000      0.948 1.000 0.000
#&gt; GSM38714     1   0.000      0.948 1.000 0.000
#&gt; GSM38715     1   0.000      0.948 1.000 0.000
#&gt; GSM38716     1   0.000      0.948 1.000 0.000
#&gt; GSM38717     1   0.000      0.948 1.000 0.000
#&gt; GSM38718     1   0.000      0.948 1.000 0.000
#&gt; GSM38719     1   0.000      0.948 1.000 0.000
#&gt; GSM38720     1   0.000      0.948 1.000 0.000
#&gt; GSM38721     1   0.000      0.948 1.000 0.000
#&gt; GSM38722     1   0.000      0.948 1.000 0.000
#&gt; GSM38723     1   0.000      0.948 1.000 0.000
#&gt; GSM38724     1   0.971      0.406 0.600 0.400
#&gt; GSM38725     1   0.000      0.948 1.000 0.000
#&gt; GSM38726     1   0.000      0.948 1.000 0.000
#&gt; GSM38727     1   0.000      0.948 1.000 0.000
#&gt; GSM38728     2   0.000      0.990 0.000 1.000
#&gt; GSM38729     1   0.000      0.948 1.000 0.000
#&gt; GSM38730     1   0.000      0.948 1.000 0.000
#&gt; GSM38731     1   0.000      0.948 1.000 0.000
#&gt; GSM38732     2   0.000      0.990 0.000 1.000
#&gt; GSM38733     1   0.000      0.948 1.000 0.000
#&gt; GSM38734     2   0.000      0.990 0.000 1.000
#&gt; GSM38735     1   0.430      0.876 0.912 0.088
#&gt; GSM38736     2   0.000      0.990 0.000 1.000
#&gt; GSM38737     2   0.000      0.990 0.000 1.000
#&gt; GSM38738     2   0.000      0.990 0.000 1.000
#&gt; GSM38739     1   0.000      0.948 1.000 0.000
#&gt; GSM38740     1   0.000      0.948 1.000 0.000
#&gt; GSM38741     2   0.000      0.990 0.000 1.000
#&gt; GSM38742     2   0.000      0.990 0.000 1.000
#&gt; GSM38743     2   0.000      0.990 0.000 1.000
#&gt; GSM38744     1   0.000      0.948 1.000 0.000
#&gt; GSM38745     1   0.141      0.933 0.980 0.020
#&gt; GSM38746     1   0.969      0.415 0.604 0.396
#&gt; GSM38747     1   0.969      0.415 0.604 0.396
#&gt; GSM38748     2   0.000      0.990 0.000 1.000
#&gt; GSM38749     1   0.000      0.948 1.000 0.000
#&gt; GSM38750     2   0.000      0.990 0.000 1.000
#&gt; GSM38751     2   0.000      0.990 0.000 1.000
#&gt; GSM38752     2   0.000      0.990 0.000 1.000
#&gt; GSM38753     2   0.000      0.990 0.000 1.000
#&gt; GSM38754     2   0.000      0.990 0.000 1.000
#&gt; GSM38755     2   0.697      0.753 0.188 0.812
#&gt; GSM38756     2   0.000      0.990 0.000 1.000
#&gt; GSM38757     2   0.000      0.990 0.000 1.000
#&gt; GSM38758     2   0.000      0.990 0.000 1.000
#&gt; GSM38759     1   0.680      0.774 0.820 0.180
#&gt; GSM38760     1   0.000      0.948 1.000 0.000
#&gt; GSM38761     2   0.000      0.990 0.000 1.000
#&gt; GSM38762     2   0.000      0.990 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38721     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38722     1  0.0237      0.984 0.996 0.004 0.000
#&gt; GSM38723     1  0.0237      0.984 0.996 0.004 0.000
#&gt; GSM38724     3  0.0237      0.947 0.004 0.000 0.996
#&gt; GSM38725     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38727     1  0.0237      0.984 0.996 0.004 0.000
#&gt; GSM38728     3  0.0237      0.947 0.004 0.000 0.996
#&gt; GSM38729     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38732     3  0.0000      0.949 0.000 0.000 1.000
#&gt; GSM38733     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM38734     3  0.0592      0.946 0.000 0.012 0.988
#&gt; GSM38735     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM38736     2  0.0237      0.982 0.000 0.996 0.004
#&gt; GSM38737     2  0.0237      0.982 0.000 0.996 0.004
#&gt; GSM38738     3  0.0000      0.949 0.000 0.000 1.000
#&gt; GSM38739     1  0.0661      0.979 0.988 0.004 0.008
#&gt; GSM38740     2  0.1031      0.964 0.024 0.976 0.000
#&gt; GSM38741     3  0.0000      0.949 0.000 0.000 1.000
#&gt; GSM38742     2  0.0237      0.982 0.000 0.996 0.004
#&gt; GSM38743     2  0.0237      0.982 0.000 0.996 0.004
#&gt; GSM38744     2  0.2959      0.880 0.100 0.900 0.000
#&gt; GSM38745     2  0.0747      0.971 0.016 0.984 0.000
#&gt; GSM38746     3  0.0475      0.946 0.004 0.004 0.992
#&gt; GSM38747     3  0.0424      0.945 0.008 0.000 0.992
#&gt; GSM38748     3  0.2537      0.900 0.000 0.080 0.920
#&gt; GSM38749     1  0.0661      0.979 0.988 0.004 0.008
#&gt; GSM38750     3  0.0000      0.949 0.000 0.000 1.000
#&gt; GSM38751     3  0.0000      0.949 0.000 0.000 1.000
#&gt; GSM38752     3  0.0592      0.946 0.000 0.012 0.988
#&gt; GSM38753     3  0.4842      0.749 0.000 0.224 0.776
#&gt; GSM38754     3  0.0747      0.944 0.000 0.016 0.984
#&gt; GSM38755     3  0.4755      0.754 0.184 0.008 0.808
#&gt; GSM38756     3  0.4842      0.749 0.000 0.224 0.776
#&gt; GSM38757     3  0.0000      0.949 0.000 0.000 1.000
#&gt; GSM38758     2  0.0237      0.982 0.000 0.996 0.004
#&gt; GSM38759     1  0.5216      0.654 0.740 0.000 0.260
#&gt; GSM38760     1  0.0475      0.982 0.992 0.004 0.004
#&gt; GSM38761     2  0.0237      0.982 0.000 0.996 0.004
#&gt; GSM38762     2  0.0237      0.982 0.000 0.996 0.004
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.1792      0.907 0.932 0.000 0.068 0.000
#&gt; GSM38714     1  0.1792      0.907 0.932 0.000 0.068 0.000
#&gt; GSM38715     1  0.1792      0.907 0.932 0.000 0.068 0.000
#&gt; GSM38716     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0188      0.935 0.996 0.000 0.004 0.000
#&gt; GSM38718     1  0.0188      0.935 0.996 0.000 0.004 0.000
#&gt; GSM38719     1  0.0188      0.935 0.996 0.000 0.004 0.000
#&gt; GSM38720     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.1792      0.907 0.932 0.000 0.068 0.000
#&gt; GSM38722     1  0.2256      0.884 0.924 0.056 0.020 0.000
#&gt; GSM38723     1  0.2565      0.875 0.912 0.056 0.032 0.000
#&gt; GSM38724     3  0.3400      0.618 0.000 0.000 0.820 0.180
#&gt; GSM38725     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.2565      0.875 0.912 0.056 0.032 0.000
#&gt; GSM38728     4  0.4560      0.745 0.004 0.000 0.296 0.700
#&gt; GSM38729     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM38732     4  0.3649      0.822 0.000 0.000 0.204 0.796
#&gt; GSM38733     1  0.1792      0.907 0.932 0.000 0.068 0.000
#&gt; GSM38734     4  0.2081      0.863 0.000 0.000 0.084 0.916
#&gt; GSM38735     2  0.0000      0.817 0.000 1.000 0.000 0.000
#&gt; GSM38736     2  0.3837      0.888 0.000 0.776 0.000 0.224
#&gt; GSM38737     2  0.3837      0.888 0.000 0.776 0.000 0.224
#&gt; GSM38738     4  0.3837      0.804 0.000 0.000 0.224 0.776
#&gt; GSM38739     3  0.5478      0.653 0.248 0.056 0.696 0.000
#&gt; GSM38740     2  0.0779      0.807 0.004 0.980 0.016 0.000
#&gt; GSM38741     4  0.3837      0.806 0.000 0.000 0.224 0.776
#&gt; GSM38742     2  0.3837      0.888 0.000 0.776 0.000 0.224
#&gt; GSM38743     2  0.3837      0.888 0.000 0.776 0.000 0.224
#&gt; GSM38744     2  0.2413      0.749 0.064 0.916 0.020 0.000
#&gt; GSM38745     2  0.0336      0.813 0.000 0.992 0.008 0.000
#&gt; GSM38746     3  0.0188      0.739 0.004 0.000 0.996 0.000
#&gt; GSM38747     3  0.0188      0.738 0.000 0.000 0.996 0.004
#&gt; GSM38748     4  0.0469      0.828 0.000 0.000 0.012 0.988
#&gt; GSM38749     3  0.5478      0.653 0.248 0.056 0.696 0.000
#&gt; GSM38750     3  0.2408      0.726 0.000 0.000 0.896 0.104
#&gt; GSM38751     3  0.2281      0.730 0.000 0.000 0.904 0.096
#&gt; GSM38752     4  0.2345      0.866 0.000 0.000 0.100 0.900
#&gt; GSM38753     4  0.0336      0.813 0.000 0.008 0.000 0.992
#&gt; GSM38754     4  0.2345      0.866 0.000 0.000 0.100 0.900
#&gt; GSM38755     3  0.6431      0.641 0.008 0.120 0.664 0.208
#&gt; GSM38756     4  0.0336      0.813 0.000 0.008 0.000 0.992
#&gt; GSM38757     3  0.3649      0.637 0.000 0.000 0.796 0.204
#&gt; GSM38758     2  0.4454      0.804 0.000 0.692 0.000 0.308
#&gt; GSM38759     1  0.7344      0.186 0.512 0.000 0.188 0.300
#&gt; GSM38760     3  0.5814      0.579 0.300 0.056 0.644 0.000
#&gt; GSM38761     2  0.3837      0.888 0.000 0.776 0.000 0.224
#&gt; GSM38762     2  0.3837      0.888 0.000 0.776 0.000 0.224
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0290      0.826 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38713     1  0.4779      0.581 0.588 0.000 0.024 0.000 0.388
#&gt; GSM38714     1  0.4779      0.581 0.588 0.000 0.024 0.000 0.388
#&gt; GSM38715     1  0.4779      0.581 0.588 0.000 0.024 0.000 0.388
#&gt; GSM38716     1  0.0162      0.826 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38717     1  0.1341      0.818 0.944 0.000 0.000 0.000 0.056
#&gt; GSM38718     1  0.3336      0.716 0.772 0.000 0.000 0.000 0.228
#&gt; GSM38719     1  0.1270      0.819 0.948 0.000 0.000 0.000 0.052
#&gt; GSM38720     1  0.1121      0.821 0.956 0.000 0.000 0.000 0.044
#&gt; GSM38721     1  0.4779      0.581 0.588 0.000 0.024 0.000 0.388
#&gt; GSM38722     1  0.1484      0.792 0.944 0.000 0.008 0.000 0.048
#&gt; GSM38723     1  0.1701      0.786 0.936 0.000 0.016 0.000 0.048
#&gt; GSM38724     3  0.6044      0.410 0.000 0.000 0.576 0.236 0.188
#&gt; GSM38725     1  0.0162      0.825 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38726     1  0.0162      0.825 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38727     1  0.1701      0.786 0.936 0.000 0.016 0.000 0.048
#&gt; GSM38728     4  0.5160      0.458 0.000 0.000 0.056 0.608 0.336
#&gt; GSM38729     1  0.0290      0.826 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38730     1  0.0162      0.825 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38731     1  0.0162      0.825 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38732     4  0.1908      0.788 0.000 0.000 0.092 0.908 0.000
#&gt; GSM38733     1  0.4779      0.581 0.588 0.000 0.024 0.000 0.388
#&gt; GSM38734     4  0.0404      0.833 0.000 0.012 0.000 0.988 0.000
#&gt; GSM38735     5  0.4708      0.475 0.000 0.436 0.016 0.000 0.548
#&gt; GSM38736     2  0.2020      0.981 0.000 0.900 0.000 0.100 0.000
#&gt; GSM38737     2  0.2020      0.981 0.000 0.900 0.000 0.100 0.000
#&gt; GSM38738     4  0.2074      0.777 0.000 0.000 0.104 0.896 0.000
#&gt; GSM38739     3  0.4548      0.607 0.232 0.000 0.716 0.000 0.052
#&gt; GSM38740     5  0.5368      0.537 0.036 0.356 0.016 0.000 0.592
#&gt; GSM38741     4  0.2179      0.780 0.000 0.000 0.100 0.896 0.004
#&gt; GSM38742     2  0.2020      0.981 0.000 0.900 0.000 0.100 0.000
#&gt; GSM38743     2  0.2020      0.981 0.000 0.900 0.000 0.100 0.000
#&gt; GSM38744     5  0.6280      0.496 0.200 0.188 0.016 0.000 0.596
#&gt; GSM38745     5  0.4822      0.501 0.004 0.416 0.016 0.000 0.564
#&gt; GSM38746     3  0.0963      0.739 0.000 0.000 0.964 0.036 0.000
#&gt; GSM38747     3  0.1469      0.733 0.000 0.000 0.948 0.036 0.016
#&gt; GSM38748     4  0.2424      0.769 0.000 0.132 0.000 0.868 0.000
#&gt; GSM38749     3  0.4509      0.606 0.236 0.000 0.716 0.000 0.048
#&gt; GSM38750     3  0.1410      0.742 0.000 0.000 0.940 0.060 0.000
#&gt; GSM38751     3  0.1341      0.742 0.000 0.000 0.944 0.056 0.000
#&gt; GSM38752     4  0.0404      0.833 0.000 0.012 0.000 0.988 0.000
#&gt; GSM38753     4  0.3074      0.702 0.000 0.196 0.000 0.804 0.000
#&gt; GSM38754     4  0.0510      0.833 0.000 0.016 0.000 0.984 0.000
#&gt; GSM38755     3  0.4648      0.602 0.024 0.004 0.716 0.244 0.012
#&gt; GSM38756     4  0.3003      0.713 0.000 0.188 0.000 0.812 0.000
#&gt; GSM38757     3  0.3508      0.609 0.000 0.000 0.748 0.252 0.000
#&gt; GSM38758     2  0.2813      0.888 0.000 0.832 0.000 0.168 0.000
#&gt; GSM38759     5  0.7260     -0.306 0.344 0.000 0.028 0.224 0.404
#&gt; GSM38760     3  0.5128      0.476 0.344 0.000 0.604 0.000 0.052
#&gt; GSM38761     2  0.2020      0.981 0.000 0.900 0.000 0.100 0.000
#&gt; GSM38762     2  0.2020      0.981 0.000 0.900 0.000 0.100 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1075      0.794 0.952 0.000 0.000 0.000 0.000 0.048
#&gt; GSM38713     6  0.3198      0.721 0.260 0.000 0.000 0.000 0.000 0.740
#&gt; GSM38714     6  0.3198      0.721 0.260 0.000 0.000 0.000 0.000 0.740
#&gt; GSM38715     6  0.3198      0.721 0.260 0.000 0.000 0.000 0.000 0.740
#&gt; GSM38716     1  0.0937      0.797 0.960 0.000 0.000 0.000 0.000 0.040
#&gt; GSM38717     1  0.2793      0.654 0.800 0.000 0.000 0.000 0.000 0.200
#&gt; GSM38718     1  0.3737      0.209 0.608 0.000 0.000 0.000 0.000 0.392
#&gt; GSM38719     1  0.2597      0.687 0.824 0.000 0.000 0.000 0.000 0.176
#&gt; GSM38720     1  0.2340      0.717 0.852 0.000 0.000 0.000 0.000 0.148
#&gt; GSM38721     6  0.3314      0.721 0.256 0.000 0.000 0.000 0.004 0.740
#&gt; GSM38722     1  0.2673      0.724 0.856 0.000 0.008 0.004 0.004 0.128
#&gt; GSM38723     1  0.2673      0.724 0.856 0.000 0.008 0.004 0.004 0.128
#&gt; GSM38724     6  0.5000     -0.103 0.000 0.000 0.464 0.032 0.020 0.484
#&gt; GSM38725     1  0.0000      0.803 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.803 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.2673      0.724 0.856 0.000 0.008 0.004 0.004 0.128
#&gt; GSM38728     6  0.5056      0.197 0.000 0.000 0.048 0.380 0.016 0.556
#&gt; GSM38729     1  0.1141      0.792 0.948 0.000 0.000 0.000 0.000 0.052
#&gt; GSM38730     1  0.0000      0.803 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.803 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     4  0.0622      0.882 0.000 0.000 0.012 0.980 0.000 0.008
#&gt; GSM38733     6  0.3337      0.719 0.260 0.000 0.000 0.000 0.004 0.736
#&gt; GSM38734     4  0.0405      0.887 0.000 0.008 0.004 0.988 0.000 0.000
#&gt; GSM38735     5  0.0937      0.983 0.000 0.040 0.000 0.000 0.960 0.000
#&gt; GSM38736     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38738     4  0.1088      0.874 0.000 0.000 0.016 0.960 0.000 0.024
#&gt; GSM38739     3  0.5853      0.420 0.304 0.000 0.544 0.004 0.016 0.132
#&gt; GSM38740     5  0.0972      0.986 0.008 0.028 0.000 0.000 0.964 0.000
#&gt; GSM38741     4  0.1867      0.850 0.000 0.000 0.036 0.924 0.004 0.036
#&gt; GSM38742     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.1003      0.978 0.016 0.020 0.000 0.000 0.964 0.000
#&gt; GSM38745     5  0.0865      0.986 0.000 0.036 0.000 0.000 0.964 0.000
#&gt; GSM38746     3  0.0976      0.699 0.000 0.000 0.968 0.008 0.008 0.016
#&gt; GSM38747     3  0.2525      0.659 0.000 0.000 0.876 0.012 0.012 0.100
#&gt; GSM38748     4  0.2697      0.817 0.000 0.188 0.000 0.812 0.000 0.000
#&gt; GSM38749     3  0.5853      0.420 0.304 0.000 0.544 0.004 0.016 0.132
#&gt; GSM38750     3  0.1578      0.713 0.000 0.000 0.936 0.048 0.004 0.012
#&gt; GSM38751     3  0.1007      0.714 0.000 0.000 0.956 0.044 0.000 0.000
#&gt; GSM38752     4  0.0717      0.889 0.000 0.016 0.000 0.976 0.000 0.008
#&gt; GSM38753     4  0.2941      0.788 0.000 0.220 0.000 0.780 0.000 0.000
#&gt; GSM38754     4  0.0717      0.889 0.000 0.016 0.000 0.976 0.000 0.008
#&gt; GSM38755     3  0.6149      0.517 0.032 0.000 0.584 0.268 0.040 0.076
#&gt; GSM38756     4  0.2883      0.796 0.000 0.212 0.000 0.788 0.000 0.000
#&gt; GSM38757     3  0.3933      0.596 0.000 0.000 0.740 0.220 0.008 0.032
#&gt; GSM38758     2  0.0547      0.973 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; GSM38759     6  0.5182      0.610 0.108 0.000 0.024 0.160 0.012 0.696
#&gt; GSM38760     1  0.6232     -0.275 0.420 0.000 0.412 0.004 0.024 0.140
#&gt; GSM38761     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      0.996 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n individual(p) k
#> MAD:skmeans 48      0.038273 2
#> MAD:skmeans 51      0.000385 3
#> MAD:skmeans 50      0.001025 4
#> MAD:skmeans 45      0.002264 5
#> MAD:skmeans 45      0.000476 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.841           0.872       0.949         0.4287 0.561   0.561
#> 3 3 0.674           0.833       0.928         0.5095 0.707   0.514
#> 4 4 0.761           0.824       0.889         0.0950 0.865   0.650
#> 5 5 0.944           0.958       0.978         0.0553 0.965   0.879
#> 6 6 0.925           0.903       0.956         0.1023 0.870   0.535
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 5
```

There is also optional best $k$ = 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000     0.9632 1.000 0.000
#&gt; GSM38713     1   0.000     0.9632 1.000 0.000
#&gt; GSM38714     1   0.000     0.9632 1.000 0.000
#&gt; GSM38715     1   0.000     0.9632 1.000 0.000
#&gt; GSM38716     1   0.000     0.9632 1.000 0.000
#&gt; GSM38717     1   0.000     0.9632 1.000 0.000
#&gt; GSM38718     1   0.000     0.9632 1.000 0.000
#&gt; GSM38719     1   0.000     0.9632 1.000 0.000
#&gt; GSM38720     1   0.000     0.9632 1.000 0.000
#&gt; GSM38721     1   0.000     0.9632 1.000 0.000
#&gt; GSM38722     1   0.000     0.9632 1.000 0.000
#&gt; GSM38723     1   0.000     0.9632 1.000 0.000
#&gt; GSM38724     1   0.000     0.9632 1.000 0.000
#&gt; GSM38725     1   0.000     0.9632 1.000 0.000
#&gt; GSM38726     1   0.000     0.9632 1.000 0.000
#&gt; GSM38727     1   0.000     0.9632 1.000 0.000
#&gt; GSM38728     1   0.000     0.9632 1.000 0.000
#&gt; GSM38729     1   0.000     0.9632 1.000 0.000
#&gt; GSM38730     1   0.000     0.9632 1.000 0.000
#&gt; GSM38731     1   0.000     0.9632 1.000 0.000
#&gt; GSM38732     1   0.000     0.9632 1.000 0.000
#&gt; GSM38733     1   0.000     0.9632 1.000 0.000
#&gt; GSM38734     2   0.995     0.2287 0.460 0.540
#&gt; GSM38735     2   0.850     0.6343 0.276 0.724
#&gt; GSM38736     2   0.000     0.8944 0.000 1.000
#&gt; GSM38737     2   0.000     0.8944 0.000 1.000
#&gt; GSM38738     1   0.343     0.9008 0.936 0.064
#&gt; GSM38739     1   0.000     0.9632 1.000 0.000
#&gt; GSM38740     1   0.949     0.3518 0.632 0.368
#&gt; GSM38741     2   0.929     0.5274 0.344 0.656
#&gt; GSM38742     2   0.000     0.8944 0.000 1.000
#&gt; GSM38743     2   0.000     0.8944 0.000 1.000
#&gt; GSM38744     1   0.000     0.9632 1.000 0.000
#&gt; GSM38745     2   0.946     0.4786 0.364 0.636
#&gt; GSM38746     1   0.327     0.9051 0.940 0.060
#&gt; GSM38747     1   0.000     0.9632 1.000 0.000
#&gt; GSM38748     2   0.000     0.8944 0.000 1.000
#&gt; GSM38749     1   0.000     0.9632 1.000 0.000
#&gt; GSM38750     1   0.615     0.7889 0.848 0.152
#&gt; GSM38751     1   0.994     0.0276 0.544 0.456
#&gt; GSM38752     2   0.204     0.8750 0.032 0.968
#&gt; GSM38753     2   0.000     0.8944 0.000 1.000
#&gt; GSM38754     2   0.000     0.8944 0.000 1.000
#&gt; GSM38755     1   0.000     0.9632 1.000 0.000
#&gt; GSM38756     2   0.000     0.8944 0.000 1.000
#&gt; GSM38757     1   0.000     0.9632 1.000 0.000
#&gt; GSM38758     2   0.000     0.8944 0.000 1.000
#&gt; GSM38759     1   0.000     0.9632 1.000 0.000
#&gt; GSM38760     1   0.000     0.9632 1.000 0.000
#&gt; GSM38761     2   0.000     0.8944 0.000 1.000
#&gt; GSM38762     2   0.000     0.8944 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38713     1  0.0237      0.914 0.996 0.000 0.004
#&gt; GSM38714     1  0.3619      0.802 0.864 0.000 0.136
#&gt; GSM38715     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38721     1  0.2878      0.841 0.904 0.000 0.096
#&gt; GSM38722     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38724     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38725     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38728     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38729     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38732     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38733     1  0.0592      0.908 0.988 0.000 0.012
#&gt; GSM38734     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38735     2  0.7412      0.670 0.176 0.700 0.124
#&gt; GSM38736     2  0.0000      0.897 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      0.897 0.000 1.000 0.000
#&gt; GSM38738     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38739     1  0.5905      0.452 0.648 0.000 0.352
#&gt; GSM38740     1  0.7915      0.457 0.644 0.248 0.108
#&gt; GSM38741     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38742     2  0.0000      0.897 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000      0.897 0.000 1.000 0.000
#&gt; GSM38744     1  0.0000      0.916 1.000 0.000 0.000
#&gt; GSM38745     2  0.7365      0.670 0.188 0.700 0.112
#&gt; GSM38746     3  0.2878      0.832 0.096 0.000 0.904
#&gt; GSM38747     3  0.2959      0.828 0.100 0.000 0.900
#&gt; GSM38748     3  0.4555      0.725 0.000 0.200 0.800
#&gt; GSM38749     1  0.5905      0.452 0.648 0.000 0.352
#&gt; GSM38750     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38751     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38752     3  0.3267      0.822 0.000 0.116 0.884
#&gt; GSM38753     2  0.4346      0.755 0.000 0.816 0.184
#&gt; GSM38754     3  0.3551      0.806 0.000 0.132 0.868
#&gt; GSM38755     3  0.0592      0.903 0.012 0.000 0.988
#&gt; GSM38756     2  0.4235      0.764 0.000 0.824 0.176
#&gt; GSM38757     3  0.0000      0.910 0.000 0.000 1.000
#&gt; GSM38758     2  0.0000      0.897 0.000 1.000 0.000
#&gt; GSM38759     3  0.6111      0.290 0.396 0.000 0.604
#&gt; GSM38760     1  0.5905      0.452 0.648 0.000 0.352
#&gt; GSM38761     2  0.0000      0.897 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000      0.897 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38714     3  0.3801      0.675 0.220 0.000 0.780 0.000
#&gt; GSM38715     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38721     3  0.4431      0.564 0.304 0.000 0.696 0.000
#&gt; GSM38722     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.0000      0.900 0.000 0.000 1.000 0.000
#&gt; GSM38725     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38728     3  0.0000      0.900 0.000 0.000 1.000 0.000
#&gt; GSM38729     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.1474      0.868 0.000 0.000 0.948 0.052
#&gt; GSM38733     1  0.1940      0.883 0.924 0.000 0.076 0.000
#&gt; GSM38734     4  0.4406      0.677 0.000 0.000 0.300 0.700
#&gt; GSM38735     2  0.0000      0.617 0.000 1.000 0.000 0.000
#&gt; GSM38736     2  0.4406      0.800 0.000 0.700 0.000 0.300
#&gt; GSM38737     2  0.4406      0.800 0.000 0.700 0.000 0.300
#&gt; GSM38738     3  0.1474      0.868 0.000 0.000 0.948 0.052
#&gt; GSM38739     1  0.3074      0.807 0.848 0.000 0.152 0.000
#&gt; GSM38740     2  0.7188     -0.192 0.428 0.436 0.136 0.000
#&gt; GSM38741     3  0.1940      0.837 0.000 0.000 0.924 0.076
#&gt; GSM38742     2  0.4406      0.800 0.000 0.700 0.000 0.300
#&gt; GSM38743     2  0.4406      0.800 0.000 0.700 0.000 0.300
#&gt; GSM38744     1  0.4406      0.645 0.700 0.300 0.000 0.000
#&gt; GSM38745     2  0.0000      0.617 0.000 1.000 0.000 0.000
#&gt; GSM38746     3  0.0000      0.900 0.000 0.000 1.000 0.000
#&gt; GSM38747     3  0.0000      0.900 0.000 0.000 1.000 0.000
#&gt; GSM38748     4  0.3444      0.825 0.000 0.000 0.184 0.816
#&gt; GSM38749     1  0.3074      0.807 0.848 0.000 0.152 0.000
#&gt; GSM38750     3  0.0000      0.900 0.000 0.000 1.000 0.000
#&gt; GSM38751     3  0.0000      0.900 0.000 0.000 1.000 0.000
#&gt; GSM38752     4  0.3528      0.822 0.000 0.000 0.192 0.808
#&gt; GSM38753     4  0.0707      0.663 0.000 0.020 0.000 0.980
#&gt; GSM38754     4  0.3219      0.822 0.000 0.000 0.164 0.836
#&gt; GSM38755     3  0.0000      0.900 0.000 0.000 1.000 0.000
#&gt; GSM38756     4  0.1557      0.618 0.000 0.056 0.000 0.944
#&gt; GSM38757     3  0.0000      0.900 0.000 0.000 1.000 0.000
#&gt; GSM38758     2  0.4406      0.800 0.000 0.700 0.000 0.300
#&gt; GSM38759     3  0.3074      0.747 0.152 0.000 0.848 0.000
#&gt; GSM38760     1  0.3074      0.807 0.848 0.000 0.152 0.000
#&gt; GSM38761     2  0.4406      0.800 0.000 0.700 0.000 0.300
#&gt; GSM38762     2  0.4406      0.800 0.000 0.700 0.000 0.300
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4 p5
#&gt; GSM38712     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38713     1  0.0162      0.987 0.996 0.00 0.004 0.000  0
#&gt; GSM38714     3  0.0162      0.956 0.004 0.00 0.996 0.000  0
#&gt; GSM38715     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38716     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38717     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38718     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38719     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38720     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38721     3  0.2179      0.837 0.112 0.00 0.888 0.000  0
#&gt; GSM38722     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38723     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38724     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38725     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38726     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38727     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38728     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38729     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38730     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38731     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38732     3  0.1908      0.902 0.000 0.00 0.908 0.092  0
#&gt; GSM38733     1  0.2424      0.827 0.868 0.00 0.132 0.000  0
#&gt; GSM38734     4  0.0000      0.886 0.000 0.00 0.000 1.000  0
#&gt; GSM38735     5  0.0000      1.000 0.000 0.00 0.000 0.000  1
#&gt; GSM38736     2  0.0000      1.000 0.000 1.00 0.000 0.000  0
#&gt; GSM38737     2  0.0000      1.000 0.000 1.00 0.000 0.000  0
#&gt; GSM38738     3  0.1908      0.902 0.000 0.00 0.908 0.092  0
#&gt; GSM38739     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38740     5  0.0000      1.000 0.000 0.00 0.000 0.000  1
#&gt; GSM38741     3  0.3039      0.781 0.000 0.00 0.808 0.192  0
#&gt; GSM38742     2  0.0000      1.000 0.000 1.00 0.000 0.000  0
#&gt; GSM38743     2  0.0000      1.000 0.000 1.00 0.000 0.000  0
#&gt; GSM38744     5  0.0000      1.000 0.000 0.00 0.000 0.000  1
#&gt; GSM38745     5  0.0000      1.000 0.000 0.00 0.000 0.000  1
#&gt; GSM38746     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38747     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38748     4  0.1732      0.881 0.000 0.08 0.000 0.920  0
#&gt; GSM38749     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38750     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38751     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38752     4  0.0000      0.886 0.000 0.00 0.000 1.000  0
#&gt; GSM38753     4  0.3109      0.820 0.000 0.20 0.000 0.800  0
#&gt; GSM38754     4  0.0000      0.886 0.000 0.00 0.000 1.000  0
#&gt; GSM38755     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38756     4  0.3109      0.820 0.000 0.20 0.000 0.800  0
#&gt; GSM38757     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38758     2  0.0000      1.000 0.000 1.00 0.000 0.000  0
#&gt; GSM38759     3  0.0000      0.959 0.000 0.00 1.000 0.000  0
#&gt; GSM38760     1  0.0000      0.991 1.000 0.00 0.000 0.000  0
#&gt; GSM38761     2  0.0000      1.000 0.000 1.00 0.000 0.000  0
#&gt; GSM38762     2  0.0000      1.000 0.000 1.00 0.000 0.000  0
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM38712     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38713     6  0.0260      0.992 0.008 0.000 0.000 0.000  0 0.992
#&gt; GSM38714     6  0.0260      0.993 0.000 0.000 0.008 0.000  0 0.992
#&gt; GSM38715     6  0.0260      0.992 0.008 0.000 0.000 0.000  0 0.992
#&gt; GSM38716     1  0.3076      0.677 0.760 0.000 0.000 0.000  0 0.240
#&gt; GSM38717     1  0.3672      0.453 0.632 0.000 0.000 0.000  0 0.368
#&gt; GSM38718     6  0.0260      0.992 0.008 0.000 0.000 0.000  0 0.992
#&gt; GSM38719     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38720     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38721     6  0.0260      0.993 0.000 0.000 0.008 0.000  0 0.992
#&gt; GSM38722     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38723     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38724     6  0.0363      0.990 0.000 0.000 0.012 0.000  0 0.988
#&gt; GSM38725     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38726     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38727     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38728     6  0.0260      0.993 0.000 0.000 0.008 0.000  0 0.992
#&gt; GSM38729     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38730     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38731     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38732     3  0.2595      0.794 0.000 0.000 0.836 0.004  0 0.160
#&gt; GSM38733     6  0.0260      0.992 0.008 0.000 0.000 0.000  0 0.992
#&gt; GSM38734     4  0.0000      0.841 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM38735     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM38738     3  0.0146      0.941 0.000 0.000 0.996 0.004  0 0.000
#&gt; GSM38739     3  0.0260      0.935 0.008 0.000 0.992 0.000  0 0.000
#&gt; GSM38740     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM38741     4  0.3244      0.584 0.000 0.000 0.268 0.732  0 0.000
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM38744     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM38745     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM38746     3  0.0000      0.942 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM38747     3  0.2730      0.738 0.000 0.000 0.808 0.000  0 0.192
#&gt; GSM38748     4  0.2257      0.831 0.000 0.116 0.000 0.876  0 0.008
#&gt; GSM38749     1  0.3737      0.355 0.608 0.000 0.392 0.000  0 0.000
#&gt; GSM38750     3  0.0000      0.942 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM38751     3  0.0000      0.942 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM38752     4  0.0000      0.841 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM38753     4  0.3043      0.784 0.000 0.200 0.000 0.792  0 0.008
#&gt; GSM38754     4  0.0000      0.841 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM38755     3  0.0000      0.942 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM38756     4  0.3043      0.784 0.000 0.200 0.000 0.792  0 0.008
#&gt; GSM38757     3  0.0000      0.942 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM38758     2  0.0000      1.000 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM38759     6  0.0260      0.993 0.000 0.000 0.008 0.000  0 0.992
#&gt; GSM38760     1  0.0000      0.918 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000 0.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n individual(p) k
#> MAD:pam 47      0.045619 2
#> MAD:pam 46      0.003165 3
#> MAD:pam 50      0.017650 4
#> MAD:pam 51      0.003717 5
#> MAD:pam 49      0.000743 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.949       0.981         0.4650 0.534   0.534
#> 3 3 0.746           0.900       0.925         0.3586 0.654   0.434
#> 4 4 0.866           0.907       0.954         0.1468 0.939   0.821
#> 5 5 0.771           0.574       0.820         0.0906 0.929   0.749
#> 6 6 0.818           0.756       0.825         0.0405 0.896   0.574
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.985 1.000 0.000
#&gt; GSM38713     1   0.000      0.985 1.000 0.000
#&gt; GSM38714     1   0.000      0.985 1.000 0.000
#&gt; GSM38715     1   0.000      0.985 1.000 0.000
#&gt; GSM38716     1   0.000      0.985 1.000 0.000
#&gt; GSM38717     1   0.000      0.985 1.000 0.000
#&gt; GSM38718     1   0.000      0.985 1.000 0.000
#&gt; GSM38719     1   0.000      0.985 1.000 0.000
#&gt; GSM38720     1   0.000      0.985 1.000 0.000
#&gt; GSM38721     1   0.000      0.985 1.000 0.000
#&gt; GSM38722     1   0.000      0.985 1.000 0.000
#&gt; GSM38723     1   0.000      0.985 1.000 0.000
#&gt; GSM38724     1   0.000      0.985 1.000 0.000
#&gt; GSM38725     1   0.000      0.985 1.000 0.000
#&gt; GSM38726     1   0.000      0.985 1.000 0.000
#&gt; GSM38727     1   0.000      0.985 1.000 0.000
#&gt; GSM38728     1   0.000      0.985 1.000 0.000
#&gt; GSM38729     1   0.995      0.091 0.540 0.460
#&gt; GSM38730     1   0.000      0.985 1.000 0.000
#&gt; GSM38731     1   0.000      0.985 1.000 0.000
#&gt; GSM38732     1   0.000      0.985 1.000 0.000
#&gt; GSM38733     1   0.000      0.985 1.000 0.000
#&gt; GSM38734     2   0.184      0.954 0.028 0.972
#&gt; GSM38735     2   0.000      0.970 0.000 1.000
#&gt; GSM38736     2   0.000      0.970 0.000 1.000
#&gt; GSM38737     2   0.000      0.970 0.000 1.000
#&gt; GSM38738     1   0.000      0.985 1.000 0.000
#&gt; GSM38739     1   0.000      0.985 1.000 0.000
#&gt; GSM38740     2   0.000      0.970 0.000 1.000
#&gt; GSM38741     1   0.000      0.985 1.000 0.000
#&gt; GSM38742     2   0.000      0.970 0.000 1.000
#&gt; GSM38743     2   0.000      0.970 0.000 1.000
#&gt; GSM38744     2   0.000      0.970 0.000 1.000
#&gt; GSM38745     2   0.000      0.970 0.000 1.000
#&gt; GSM38746     1   0.000      0.985 1.000 0.000
#&gt; GSM38747     1   0.000      0.985 1.000 0.000
#&gt; GSM38748     2   0.184      0.954 0.028 0.972
#&gt; GSM38749     1   0.000      0.985 1.000 0.000
#&gt; GSM38750     1   0.000      0.985 1.000 0.000
#&gt; GSM38751     1   0.000      0.985 1.000 0.000
#&gt; GSM38752     2   0.141      0.960 0.020 0.980
#&gt; GSM38753     2   0.000      0.970 0.000 1.000
#&gt; GSM38754     2   0.141      0.960 0.020 0.980
#&gt; GSM38755     1   0.000      0.985 1.000 0.000
#&gt; GSM38756     2   0.000      0.970 0.000 1.000
#&gt; GSM38757     1   0.000      0.985 1.000 0.000
#&gt; GSM38758     2   0.000      0.970 0.000 1.000
#&gt; GSM38759     2   0.969      0.338 0.396 0.604
#&gt; GSM38760     1   0.000      0.985 1.000 0.000
#&gt; GSM38761     2   0.000      0.970 0.000 1.000
#&gt; GSM38762     2   0.000      0.970 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38720     1  0.0237      0.990 0.996 0.000 0.004
#&gt; GSM38721     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38722     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38724     3  0.6215      0.529 0.428 0.000 0.572
#&gt; GSM38725     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38728     1  0.1753      0.950 0.952 0.000 0.048
#&gt; GSM38729     1  0.1015      0.975 0.980 0.008 0.012
#&gt; GSM38730     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38732     3  0.5216      0.810 0.260 0.000 0.740
#&gt; GSM38733     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM38734     3  0.0424      0.745 0.000 0.008 0.992
#&gt; GSM38735     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38738     3  0.5138      0.813 0.252 0.000 0.748
#&gt; GSM38739     3  0.4931      0.819 0.232 0.000 0.768
#&gt; GSM38740     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38741     3  0.1643      0.773 0.044 0.000 0.956
#&gt; GSM38742     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38744     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38745     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38746     3  0.5216      0.810 0.260 0.000 0.740
#&gt; GSM38747     3  0.5254      0.807 0.264 0.000 0.736
#&gt; GSM38748     3  0.4452      0.588 0.000 0.192 0.808
#&gt; GSM38749     3  0.4931      0.819 0.232 0.000 0.768
#&gt; GSM38750     3  0.5178      0.812 0.256 0.000 0.744
#&gt; GSM38751     3  0.2537      0.790 0.080 0.000 0.920
#&gt; GSM38752     3  0.0424      0.745 0.000 0.008 0.992
#&gt; GSM38753     3  0.4654      0.567 0.000 0.208 0.792
#&gt; GSM38754     3  0.0424      0.745 0.000 0.008 0.992
#&gt; GSM38755     3  0.5254      0.807 0.264 0.000 0.736
#&gt; GSM38756     3  0.4654      0.567 0.000 0.208 0.792
#&gt; GSM38757     3  0.4931      0.819 0.232 0.000 0.768
#&gt; GSM38758     2  0.0237      0.997 0.000 0.996 0.004
#&gt; GSM38759     1  0.2063      0.947 0.948 0.008 0.044
#&gt; GSM38760     3  0.5254      0.807 0.264 0.000 0.736
#&gt; GSM38761     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.2868      0.846 0.864 0.000 0.000 0.136
#&gt; GSM38714     1  0.3219      0.822 0.836 0.000 0.000 0.164
#&gt; GSM38715     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.2868      0.846 0.864 0.000 0.000 0.136
#&gt; GSM38722     1  0.0188      0.930 0.996 0.000 0.004 0.000
#&gt; GSM38723     1  0.0188      0.930 0.996 0.000 0.004 0.000
#&gt; GSM38724     3  0.0188      0.970 0.004 0.000 0.996 0.000
#&gt; GSM38725     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0188      0.930 0.996 0.000 0.004 0.000
#&gt; GSM38728     1  0.6595      0.563 0.628 0.000 0.160 0.212
#&gt; GSM38729     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.933 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.4446      0.654 0.196 0.000 0.776 0.028
#&gt; GSM38733     1  0.5722      0.690 0.716 0.000 0.148 0.136
#&gt; GSM38734     4  0.1792      0.868 0.000 0.000 0.068 0.932
#&gt; GSM38735     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38736     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38738     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38739     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38740     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38741     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38742     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38743     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38744     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38745     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38746     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38747     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38748     4  0.2706      0.864 0.000 0.020 0.080 0.900
#&gt; GSM38749     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38750     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38751     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38752     4  0.0000      0.879 0.000 0.000 0.000 1.000
#&gt; GSM38753     4  0.3610      0.784 0.000 0.200 0.000 0.800
#&gt; GSM38754     4  0.0000      0.879 0.000 0.000 0.000 1.000
#&gt; GSM38755     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38756     4  0.3610      0.784 0.000 0.200 0.000 0.800
#&gt; GSM38757     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38758     2  0.4008      0.630 0.000 0.756 0.000 0.244
#&gt; GSM38759     1  0.3726      0.771 0.788 0.000 0.000 0.212
#&gt; GSM38760     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM38761     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM38762     2  0.0000      0.972 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.4161    -0.4029 0.608 0.000 0.000 0.000 0.392
#&gt; GSM38713     5  0.4171    -0.2328 0.396 0.000 0.000 0.000 0.604
#&gt; GSM38714     1  0.4045     0.3717 0.644 0.000 0.000 0.000 0.356
#&gt; GSM38715     5  0.4201     0.0216 0.408 0.000 0.000 0.000 0.592
#&gt; GSM38716     1  0.4171    -0.4123 0.604 0.000 0.000 0.000 0.396
#&gt; GSM38717     1  0.0000     0.2482 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.3366     0.3410 0.768 0.000 0.000 0.000 0.232
#&gt; GSM38719     1  0.3837    -0.2942 0.692 0.000 0.000 0.000 0.308
#&gt; GSM38720     1  0.0000     0.2482 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.4114     0.3684 0.624 0.000 0.000 0.000 0.376
#&gt; GSM38722     5  0.4291     0.5769 0.464 0.000 0.000 0.000 0.536
#&gt; GSM38723     5  0.4291     0.5769 0.464 0.000 0.000 0.000 0.536
#&gt; GSM38724     3  0.6582     0.5834 0.092 0.000 0.628 0.120 0.160
#&gt; GSM38725     5  0.4307     0.5347 0.496 0.000 0.000 0.000 0.504
#&gt; GSM38726     1  0.4192    -0.4247 0.596 0.000 0.000 0.000 0.404
#&gt; GSM38727     5  0.4291     0.5769 0.464 0.000 0.000 0.000 0.536
#&gt; GSM38728     1  0.4225     0.3696 0.632 0.000 0.004 0.000 0.364
#&gt; GSM38729     1  0.0000     0.2482 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.4192    -0.4242 0.596 0.000 0.000 0.000 0.404
#&gt; GSM38731     1  0.3966    -0.3250 0.664 0.000 0.000 0.000 0.336
#&gt; GSM38732     3  0.6101     0.6065 0.156 0.000 0.664 0.124 0.056
#&gt; GSM38733     1  0.4126     0.3663 0.620 0.000 0.000 0.000 0.380
#&gt; GSM38734     4  0.0000     0.9096 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38735     2  0.2127     0.9120 0.000 0.892 0.000 0.000 0.108
#&gt; GSM38736     2  0.0000     0.9351 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000     0.9351 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38738     3  0.1121     0.9176 0.000 0.000 0.956 0.000 0.044
#&gt; GSM38739     3  0.0000     0.9333 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38740     2  0.2127     0.9120 0.000 0.892 0.000 0.000 0.108
#&gt; GSM38741     3  0.0000     0.9333 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38742     2  0.0000     0.9351 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000     0.9351 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     2  0.2127     0.9120 0.000 0.892 0.000 0.000 0.108
#&gt; GSM38745     2  0.2127     0.9120 0.000 0.892 0.000 0.000 0.108
#&gt; GSM38746     3  0.0000     0.9333 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38747     3  0.0000     0.9333 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38748     4  0.0162     0.9091 0.000 0.004 0.000 0.996 0.000
#&gt; GSM38749     3  0.0000     0.9333 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38750     3  0.0000     0.9333 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38751     3  0.0000     0.9333 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38752     4  0.0000     0.9096 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38753     4  0.3109     0.7837 0.000 0.200 0.000 0.800 0.000
#&gt; GSM38754     4  0.0000     0.9096 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38755     3  0.1121     0.9176 0.000 0.000 0.956 0.000 0.044
#&gt; GSM38756     4  0.3109     0.7837 0.000 0.200 0.000 0.800 0.000
#&gt; GSM38757     3  0.0000     0.9333 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38758     2  0.3210     0.6785 0.000 0.788 0.000 0.212 0.000
#&gt; GSM38759     1  0.4045     0.3717 0.644 0.000 0.000 0.000 0.356
#&gt; GSM38760     3  0.1121     0.9176 0.000 0.000 0.956 0.000 0.044
#&gt; GSM38761     2  0.0000     0.9351 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000     0.9351 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.2219     0.7600 0.864 0.000 0.000 0.000 0.000 0.136
#&gt; GSM38713     6  0.2762     0.6854 0.196 0.000 0.000 0.000 0.000 0.804
#&gt; GSM38714     6  0.1444     0.7551 0.072 0.000 0.000 0.000 0.000 0.928
#&gt; GSM38715     6  0.3838     0.1772 0.448 0.000 0.000 0.000 0.000 0.552
#&gt; GSM38716     1  0.2092     0.7660 0.876 0.000 0.000 0.000 0.000 0.124
#&gt; GSM38717     6  0.3330     0.6667 0.284 0.000 0.000 0.000 0.000 0.716
#&gt; GSM38718     6  0.2912     0.7181 0.216 0.000 0.000 0.000 0.000 0.784
#&gt; GSM38719     1  0.3868    -0.1550 0.508 0.000 0.000 0.000 0.000 0.492
#&gt; GSM38720     6  0.3330     0.6667 0.284 0.000 0.000 0.000 0.000 0.716
#&gt; GSM38721     6  0.1501     0.7551 0.076 0.000 0.000 0.000 0.000 0.924
#&gt; GSM38722     1  0.0000     0.7819 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000     0.7819 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.5108     0.7847 0.000 0.264 0.620 0.004 0.000 0.112
#&gt; GSM38725     1  0.1267     0.8019 0.940 0.000 0.000 0.000 0.000 0.060
#&gt; GSM38726     1  0.1663     0.7946 0.912 0.000 0.000 0.000 0.000 0.088
#&gt; GSM38727     1  0.0000     0.7819 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38728     6  0.3503     0.5723 0.000 0.020 0.012 0.180 0.000 0.788
#&gt; GSM38729     6  0.3330     0.6667 0.284 0.000 0.000 0.000 0.000 0.716
#&gt; GSM38730     1  0.1267     0.8019 0.940 0.000 0.000 0.000 0.000 0.060
#&gt; GSM38731     1  0.3847    -0.0252 0.544 0.000 0.000 0.000 0.000 0.456
#&gt; GSM38732     3  0.6011     0.6948 0.000 0.264 0.532 0.184 0.000 0.020
#&gt; GSM38733     6  0.0458     0.7313 0.016 0.000 0.000 0.000 0.000 0.984
#&gt; GSM38734     4  0.0820     0.8719 0.000 0.016 0.000 0.972 0.000 0.012
#&gt; GSM38735     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38736     2  0.3756     0.8674 0.000 0.600 0.000 0.000 0.400 0.000
#&gt; GSM38737     2  0.3756     0.8674 0.000 0.600 0.000 0.000 0.400 0.000
#&gt; GSM38738     3  0.3905     0.8312 0.000 0.264 0.712 0.012 0.000 0.012
#&gt; GSM38739     3  0.0000     0.8283 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38740     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38741     3  0.4822     0.8081 0.000 0.264 0.656 0.068 0.000 0.012
#&gt; GSM38742     2  0.3717     0.8620 0.000 0.616 0.000 0.000 0.384 0.000
#&gt; GSM38743     2  0.3756     0.8674 0.000 0.600 0.000 0.000 0.400 0.000
#&gt; GSM38744     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38745     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38746     3  0.0000     0.8283 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38747     3  0.3360     0.8363 0.000 0.264 0.732 0.004 0.000 0.000
#&gt; GSM38748     4  0.2454     0.8840 0.000 0.160 0.000 0.840 0.000 0.000
#&gt; GSM38749     3  0.0000     0.8283 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38750     3  0.0547     0.8320 0.000 0.020 0.980 0.000 0.000 0.000
#&gt; GSM38751     3  0.0000     0.8283 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38752     4  0.0000     0.8877 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38753     4  0.2793     0.8690 0.000 0.200 0.000 0.800 0.000 0.000
#&gt; GSM38754     4  0.0000     0.8877 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38755     3  0.4152     0.8316 0.024 0.264 0.700 0.000 0.000 0.012
#&gt; GSM38756     4  0.2793     0.8690 0.000 0.200 0.000 0.800 0.000 0.000
#&gt; GSM38757     3  0.3360     0.8363 0.000 0.264 0.732 0.004 0.000 0.000
#&gt; GSM38758     2  0.3617     0.3247 0.000 0.736 0.000 0.244 0.020 0.000
#&gt; GSM38759     6  0.0909     0.7251 0.012 0.000 0.000 0.020 0.000 0.968
#&gt; GSM38760     3  0.1267     0.7983 0.060 0.000 0.940 0.000 0.000 0.000
#&gt; GSM38761     2  0.3717     0.8620 0.000 0.616 0.000 0.000 0.384 0.000
#&gt; GSM38762     2  0.3756     0.8674 0.000 0.600 0.000 0.000 0.400 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n individual(p) k
#> MAD:mclust 49      0.025565 2
#> MAD:mclust 51      0.000095 3
#> MAD:mclust 51      0.000448 4
#> MAD:mclust 34      0.016251 5
#> MAD:mclust 47      0.000245 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.918           0.908       0.964         0.4396 0.561   0.561
#> 3 3 0.937           0.908       0.961         0.3574 0.772   0.618
#> 4 4 0.855           0.862       0.936         0.2510 0.807   0.549
#> 5 5 0.777           0.691       0.871         0.0327 0.890   0.641
#> 6 6 0.694           0.658       0.837         0.0305 0.975   0.900
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.968 1.000 0.000
#&gt; GSM38713     1  0.0000      0.968 1.000 0.000
#&gt; GSM38714     1  0.0000      0.968 1.000 0.000
#&gt; GSM38715     1  0.0000      0.968 1.000 0.000
#&gt; GSM38716     1  0.0000      0.968 1.000 0.000
#&gt; GSM38717     1  0.0000      0.968 1.000 0.000
#&gt; GSM38718     1  0.0000      0.968 1.000 0.000
#&gt; GSM38719     1  0.0000      0.968 1.000 0.000
#&gt; GSM38720     1  0.0000      0.968 1.000 0.000
#&gt; GSM38721     1  0.0000      0.968 1.000 0.000
#&gt; GSM38722     1  0.0000      0.968 1.000 0.000
#&gt; GSM38723     1  0.0000      0.968 1.000 0.000
#&gt; GSM38724     1  0.0000      0.968 1.000 0.000
#&gt; GSM38725     1  0.0000      0.968 1.000 0.000
#&gt; GSM38726     1  0.0000      0.968 1.000 0.000
#&gt; GSM38727     1  0.0000      0.968 1.000 0.000
#&gt; GSM38728     1  0.9710      0.313 0.600 0.400
#&gt; GSM38729     1  0.0000      0.968 1.000 0.000
#&gt; GSM38730     1  0.0000      0.968 1.000 0.000
#&gt; GSM38731     1  0.0000      0.968 1.000 0.000
#&gt; GSM38732     2  0.9393      0.432 0.356 0.644
#&gt; GSM38733     1  0.0000      0.968 1.000 0.000
#&gt; GSM38734     2  0.0000      0.942 0.000 1.000
#&gt; GSM38735     1  0.0938      0.959 0.988 0.012
#&gt; GSM38736     2  0.0000      0.942 0.000 1.000
#&gt; GSM38737     2  0.0000      0.942 0.000 1.000
#&gt; GSM38738     2  0.9954      0.118 0.460 0.540
#&gt; GSM38739     1  0.0000      0.968 1.000 0.000
#&gt; GSM38740     1  0.0000      0.968 1.000 0.000
#&gt; GSM38741     2  0.0000      0.942 0.000 1.000
#&gt; GSM38742     2  0.0000      0.942 0.000 1.000
#&gt; GSM38743     2  0.0000      0.942 0.000 1.000
#&gt; GSM38744     1  0.0000      0.968 1.000 0.000
#&gt; GSM38745     1  0.0000      0.968 1.000 0.000
#&gt; GSM38746     1  0.0672      0.962 0.992 0.008
#&gt; GSM38747     1  0.6438      0.793 0.836 0.164
#&gt; GSM38748     2  0.0000      0.942 0.000 1.000
#&gt; GSM38749     1  0.0000      0.968 1.000 0.000
#&gt; GSM38750     1  0.4298      0.888 0.912 0.088
#&gt; GSM38751     1  0.8499      0.610 0.724 0.276
#&gt; GSM38752     2  0.0000      0.942 0.000 1.000
#&gt; GSM38753     2  0.0000      0.942 0.000 1.000
#&gt; GSM38754     2  0.0000      0.942 0.000 1.000
#&gt; GSM38755     1  0.0000      0.968 1.000 0.000
#&gt; GSM38756     2  0.0000      0.942 0.000 1.000
#&gt; GSM38757     1  0.3274      0.917 0.940 0.060
#&gt; GSM38758     2  0.0000      0.942 0.000 1.000
#&gt; GSM38759     1  0.0000      0.968 1.000 0.000
#&gt; GSM38760     1  0.0000      0.968 1.000 0.000
#&gt; GSM38761     2  0.0000      0.942 0.000 1.000
#&gt; GSM38762     2  0.0000      0.942 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM38713     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38716     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM38717     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38721     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38722     1  0.0747      0.961 0.984 0.016 0.000
#&gt; GSM38723     1  0.0592      0.963 0.988 0.012 0.000
#&gt; GSM38724     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM38725     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38726     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM38727     1  0.0592      0.963 0.988 0.012 0.000
#&gt; GSM38728     3  0.2165      0.870 0.064 0.000 0.936
#&gt; GSM38729     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM38730     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM38731     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM38732     3  0.1529      0.893 0.040 0.000 0.960
#&gt; GSM38733     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38734     3  0.0000      0.918 0.000 0.000 1.000
#&gt; GSM38735     2  0.0000      0.950 0.000 1.000 0.000
#&gt; GSM38736     2  0.1031      0.957 0.000 0.976 0.024
#&gt; GSM38737     2  0.1031      0.957 0.000 0.976 0.024
#&gt; GSM38738     3  0.1031      0.906 0.024 0.000 0.976
#&gt; GSM38739     1  0.0592      0.963 0.988 0.012 0.000
#&gt; GSM38740     2  0.0747      0.942 0.016 0.984 0.000
#&gt; GSM38741     3  0.0000      0.918 0.000 0.000 1.000
#&gt; GSM38742     2  0.1411      0.952 0.000 0.964 0.036
#&gt; GSM38743     2  0.1031      0.957 0.000 0.976 0.024
#&gt; GSM38744     2  0.1411      0.923 0.036 0.964 0.000
#&gt; GSM38745     2  0.0000      0.950 0.000 1.000 0.000
#&gt; GSM38746     1  0.2564      0.926 0.936 0.036 0.028
#&gt; GSM38747     1  0.4399      0.766 0.812 0.000 0.188
#&gt; GSM38748     3  0.0000      0.918 0.000 0.000 1.000
#&gt; GSM38749     1  0.0424      0.965 0.992 0.008 0.000
#&gt; GSM38750     1  0.6215      0.213 0.572 0.000 0.428
#&gt; GSM38751     1  0.3918      0.850 0.868 0.012 0.120
#&gt; GSM38752     3  0.0000      0.918 0.000 0.000 1.000
#&gt; GSM38753     3  0.0000      0.918 0.000 0.000 1.000
#&gt; GSM38754     3  0.0000      0.918 0.000 0.000 1.000
#&gt; GSM38755     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM38756     3  0.0000      0.918 0.000 0.000 1.000
#&gt; GSM38757     3  0.6274      0.136 0.456 0.000 0.544
#&gt; GSM38758     2  0.5178      0.691 0.000 0.744 0.256
#&gt; GSM38759     1  0.0424      0.962 0.992 0.000 0.008
#&gt; GSM38760     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM38761     2  0.1753      0.945 0.000 0.952 0.048
#&gt; GSM38762     2  0.1031      0.957 0.000 0.976 0.024
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.2345      0.859 0.900 0.000 0.100 0.000
#&gt; GSM38713     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.2281      0.862 0.904 0.000 0.096 0.000
#&gt; GSM38717     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38722     3  0.2868      0.798 0.136 0.000 0.864 0.000
#&gt; GSM38723     3  0.0707      0.918 0.020 0.000 0.980 0.000
#&gt; GSM38724     1  0.7676      0.271 0.452 0.000 0.240 0.308
#&gt; GSM38725     1  0.3975      0.714 0.760 0.000 0.240 0.000
#&gt; GSM38726     1  0.2589      0.848 0.884 0.000 0.116 0.000
#&gt; GSM38727     3  0.0921      0.912 0.028 0.000 0.972 0.000
#&gt; GSM38728     1  0.4933      0.292 0.568 0.000 0.000 0.432
#&gt; GSM38729     1  0.0188      0.904 0.996 0.000 0.004 0.000
#&gt; GSM38730     1  0.3486      0.782 0.812 0.000 0.188 0.000
#&gt; GSM38731     1  0.0817      0.897 0.976 0.000 0.024 0.000
#&gt; GSM38732     4  0.1302      0.947 0.044 0.000 0.000 0.956
#&gt; GSM38733     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38734     4  0.0000      0.987 0.000 0.000 0.000 1.000
#&gt; GSM38735     2  0.0336      0.926 0.000 0.992 0.008 0.000
#&gt; GSM38736     2  0.0000      0.929 0.000 1.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.929 0.000 1.000 0.000 0.000
#&gt; GSM38738     4  0.0469      0.982 0.000 0.000 0.012 0.988
#&gt; GSM38739     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM38740     2  0.5536      0.389 0.024 0.592 0.384 0.000
#&gt; GSM38741     4  0.0592      0.979 0.000 0.000 0.016 0.984
#&gt; GSM38742     2  0.0000      0.929 0.000 1.000 0.000 0.000
#&gt; GSM38743     2  0.0000      0.929 0.000 1.000 0.000 0.000
#&gt; GSM38744     2  0.4996      0.708 0.056 0.752 0.192 0.000
#&gt; GSM38745     2  0.1022      0.913 0.000 0.968 0.032 0.000
#&gt; GSM38746     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM38747     3  0.2266      0.871 0.004 0.000 0.912 0.084
#&gt; GSM38748     4  0.0000      0.987 0.000 0.000 0.000 1.000
#&gt; GSM38749     3  0.0000      0.924 0.000 0.000 1.000 0.000
#&gt; GSM38750     3  0.1118      0.909 0.000 0.000 0.964 0.036
#&gt; GSM38751     3  0.0336      0.922 0.000 0.000 0.992 0.008
#&gt; GSM38752     4  0.0188      0.987 0.000 0.004 0.000 0.996
#&gt; GSM38753     4  0.0336      0.986 0.000 0.008 0.000 0.992
#&gt; GSM38754     4  0.0188      0.987 0.000 0.004 0.000 0.996
#&gt; GSM38755     3  0.0188      0.924 0.000 0.000 0.996 0.004
#&gt; GSM38756     4  0.0336      0.986 0.000 0.008 0.000 0.992
#&gt; GSM38757     3  0.4898      0.287 0.000 0.000 0.584 0.416
#&gt; GSM38758     2  0.0336      0.924 0.000 0.992 0.000 0.008
#&gt; GSM38759     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM38760     3  0.0188      0.924 0.004 0.000 0.996 0.000
#&gt; GSM38761     2  0.0000      0.929 0.000 1.000 0.000 0.000
#&gt; GSM38762     2  0.0000      0.929 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.1106     0.8100 0.964 0.000 0.024 0.000 0.012
#&gt; GSM38713     1  0.4074     0.3642 0.636 0.000 0.000 0.000 0.364
#&gt; GSM38714     1  0.0794     0.8091 0.972 0.000 0.000 0.000 0.028
#&gt; GSM38715     1  0.2424     0.7382 0.868 0.000 0.000 0.000 0.132
#&gt; GSM38716     1  0.1331     0.8065 0.952 0.000 0.040 0.000 0.008
#&gt; GSM38717     1  0.0451     0.8130 0.988 0.000 0.004 0.000 0.008
#&gt; GSM38718     1  0.1544     0.7903 0.932 0.000 0.000 0.000 0.068
#&gt; GSM38719     1  0.0404     0.8124 0.988 0.000 0.000 0.000 0.012
#&gt; GSM38720     1  0.0609     0.8113 0.980 0.000 0.000 0.000 0.020
#&gt; GSM38721     1  0.0324     0.8135 0.992 0.000 0.004 0.000 0.004
#&gt; GSM38722     1  0.4425     0.3733 0.600 0.000 0.392 0.000 0.008
#&gt; GSM38723     3  0.1410     0.8134 0.060 0.000 0.940 0.000 0.000
#&gt; GSM38724     1  0.4911     0.6260 0.736 0.000 0.132 0.124 0.008
#&gt; GSM38725     1  0.4042     0.6452 0.756 0.000 0.212 0.000 0.032
#&gt; GSM38726     1  0.1478     0.7987 0.936 0.000 0.064 0.000 0.000
#&gt; GSM38727     3  0.2179     0.7498 0.112 0.000 0.888 0.000 0.000
#&gt; GSM38728     4  0.5414     0.1119 0.412 0.000 0.000 0.528 0.060
#&gt; GSM38729     1  0.0162     0.8133 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38730     1  0.1469     0.8056 0.948 0.000 0.036 0.000 0.016
#&gt; GSM38731     1  0.0992     0.8132 0.968 0.000 0.008 0.000 0.024
#&gt; GSM38732     5  0.2423     0.3286 0.024 0.000 0.000 0.080 0.896
#&gt; GSM38733     5  0.3932     0.2585 0.328 0.000 0.000 0.000 0.672
#&gt; GSM38734     4  0.4305     0.2026 0.000 0.000 0.000 0.512 0.488
#&gt; GSM38735     2  0.1341     0.9317 0.000 0.944 0.000 0.000 0.056
#&gt; GSM38736     2  0.0000     0.9597 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000     0.9597 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38738     5  0.6191    -0.1392 0.000 0.000 0.424 0.136 0.440
#&gt; GSM38739     3  0.0000     0.8567 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38740     1  0.7324     0.1071 0.432 0.372 0.124 0.000 0.072
#&gt; GSM38741     4  0.1478     0.7231 0.000 0.000 0.064 0.936 0.000
#&gt; GSM38742     2  0.0000     0.9597 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000     0.9597 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     1  0.5949     0.0718 0.472 0.444 0.012 0.000 0.072
#&gt; GSM38745     2  0.2429     0.9007 0.020 0.904 0.008 0.000 0.068
#&gt; GSM38746     3  0.0451     0.8537 0.000 0.000 0.988 0.004 0.008
#&gt; GSM38747     3  0.3636     0.5472 0.000 0.000 0.728 0.272 0.000
#&gt; GSM38748     4  0.3003     0.6625 0.000 0.000 0.000 0.812 0.188
#&gt; GSM38749     3  0.0000     0.8567 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38750     3  0.0955     0.8426 0.000 0.000 0.968 0.004 0.028
#&gt; GSM38751     3  0.0000     0.8567 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38752     4  0.0404     0.7678 0.000 0.000 0.000 0.988 0.012
#&gt; GSM38753     4  0.0290     0.7652 0.000 0.008 0.000 0.992 0.000
#&gt; GSM38754     4  0.0898     0.7648 0.000 0.008 0.000 0.972 0.020
#&gt; GSM38755     3  0.4182     0.2314 0.000 0.000 0.600 0.000 0.400
#&gt; GSM38756     4  0.0290     0.7672 0.000 0.000 0.000 0.992 0.008
#&gt; GSM38757     3  0.3532     0.7296 0.000 0.000 0.824 0.048 0.128
#&gt; GSM38758     2  0.2690     0.8037 0.000 0.844 0.000 0.156 0.000
#&gt; GSM38759     1  0.1571     0.7943 0.936 0.000 0.000 0.004 0.060
#&gt; GSM38760     3  0.0000     0.8567 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38761     2  0.0000     0.9597 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0162     0.9583 0.000 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1267      0.796 0.940 0.000 0.000 0.000 0.000 0.060
#&gt; GSM38713     1  0.4012      0.608 0.748 0.000 0.000 0.000 0.076 0.176
#&gt; GSM38714     1  0.0717      0.797 0.976 0.000 0.000 0.000 0.008 0.016
#&gt; GSM38715     1  0.2119      0.781 0.904 0.000 0.000 0.000 0.036 0.060
#&gt; GSM38716     1  0.1408      0.796 0.944 0.000 0.020 0.000 0.000 0.036
#&gt; GSM38717     1  0.0458      0.799 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM38718     1  0.2300      0.735 0.856 0.000 0.000 0.000 0.000 0.144
#&gt; GSM38719     1  0.0363      0.798 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM38720     1  0.1007      0.790 0.956 0.000 0.000 0.000 0.000 0.044
#&gt; GSM38721     1  0.0790      0.798 0.968 0.000 0.000 0.000 0.000 0.032
#&gt; GSM38722     1  0.3943      0.657 0.756 0.000 0.184 0.000 0.004 0.056
#&gt; GSM38723     3  0.2810      0.736 0.156 0.000 0.832 0.000 0.004 0.008
#&gt; GSM38724     1  0.4616      0.681 0.756 0.000 0.028 0.084 0.012 0.120
#&gt; GSM38725     1  0.5769      0.462 0.608 0.000 0.216 0.000 0.040 0.136
#&gt; GSM38726     1  0.2249      0.779 0.900 0.000 0.064 0.000 0.004 0.032
#&gt; GSM38727     3  0.3430      0.662 0.208 0.000 0.772 0.000 0.004 0.016
#&gt; GSM38728     4  0.6489     -0.225 0.292 0.000 0.000 0.400 0.020 0.288
#&gt; GSM38729     1  0.1501      0.791 0.924 0.000 0.000 0.000 0.000 0.076
#&gt; GSM38730     1  0.1418      0.798 0.944 0.000 0.024 0.000 0.000 0.032
#&gt; GSM38731     1  0.3114      0.740 0.832 0.000 0.036 0.000 0.004 0.128
#&gt; GSM38732     5  0.4093     -0.395 0.004 0.000 0.000 0.004 0.552 0.440
#&gt; GSM38733     6  0.5438      0.000 0.148 0.000 0.000 0.000 0.304 0.548
#&gt; GSM38734     5  0.1720      0.402 0.000 0.000 0.000 0.032 0.928 0.040
#&gt; GSM38735     2  0.2994      0.760 0.004 0.788 0.000 0.000 0.000 0.208
#&gt; GSM38736     2  0.0000      0.901 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.901 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38738     5  0.3014      0.490 0.000 0.000 0.184 0.012 0.804 0.000
#&gt; GSM38739     3  0.0692      0.842 0.000 0.000 0.976 0.000 0.004 0.020
#&gt; GSM38740     1  0.7155      0.126 0.396 0.148 0.132 0.000 0.000 0.324
#&gt; GSM38741     4  0.0935      0.748 0.000 0.000 0.032 0.964 0.000 0.004
#&gt; GSM38742     2  0.0146      0.900 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38743     2  0.0000      0.901 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     1  0.6551      0.206 0.448 0.184 0.044 0.000 0.000 0.324
#&gt; GSM38745     2  0.4501      0.626 0.012 0.660 0.036 0.000 0.000 0.292
#&gt; GSM38746     3  0.1806      0.806 0.000 0.000 0.908 0.004 0.000 0.088
#&gt; GSM38747     3  0.4444      0.638 0.016 0.000 0.736 0.164 0.000 0.084
#&gt; GSM38748     5  0.4594      0.317 0.000 0.000 0.000 0.340 0.608 0.052
#&gt; GSM38749     3  0.0260      0.847 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM38750     3  0.1411      0.828 0.000 0.000 0.936 0.000 0.060 0.004
#&gt; GSM38751     3  0.0291      0.846 0.000 0.000 0.992 0.000 0.004 0.004
#&gt; GSM38752     4  0.2146      0.733 0.000 0.000 0.000 0.880 0.116 0.004
#&gt; GSM38753     4  0.0000      0.760 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38754     4  0.2520      0.709 0.000 0.000 0.000 0.844 0.152 0.004
#&gt; GSM38755     5  0.4074      0.428 0.004 0.000 0.288 0.000 0.684 0.024
#&gt; GSM38756     4  0.0777      0.754 0.000 0.004 0.000 0.972 0.024 0.000
#&gt; GSM38757     3  0.2527      0.733 0.000 0.000 0.832 0.000 0.168 0.000
#&gt; GSM38758     2  0.2631      0.728 0.000 0.820 0.000 0.180 0.000 0.000
#&gt; GSM38759     1  0.4672      0.602 0.728 0.000 0.000 0.152 0.028 0.092
#&gt; GSM38760     3  0.1268      0.838 0.036 0.000 0.952 0.000 0.008 0.004
#&gt; GSM38761     2  0.0146      0.900 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM38762     2  0.0000      0.901 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n individual(p) k
#> MAD:NMF 48      0.079586 2
#> MAD:NMF 49      0.008434 3
#> MAD:NMF 47      0.000522 4
#> MAD:NMF 41      0.001165 5
#> MAD:NMF 41      0.000245 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.758           0.957       0.977         0.4046 0.613   0.613
#> 3 3 1.000           1.000       1.000         0.3795 0.830   0.722
#> 4 4 0.793           0.935       0.916         0.1170 0.967   0.926
#> 5 5 0.780           0.826       0.873         0.1731 0.813   0.544
#> 6 6 0.761           0.843       0.847         0.0223 0.957   0.813
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.967 1.000 0.000
#&gt; GSM38713     1   0.000      0.967 1.000 0.000
#&gt; GSM38714     1   0.000      0.967 1.000 0.000
#&gt; GSM38715     1   0.000      0.967 1.000 0.000
#&gt; GSM38716     1   0.000      0.967 1.000 0.000
#&gt; GSM38717     1   0.000      0.967 1.000 0.000
#&gt; GSM38718     1   0.000      0.967 1.000 0.000
#&gt; GSM38719     1   0.000      0.967 1.000 0.000
#&gt; GSM38720     1   0.000      0.967 1.000 0.000
#&gt; GSM38721     1   0.000      0.967 1.000 0.000
#&gt; GSM38722     1   0.000      0.967 1.000 0.000
#&gt; GSM38723     1   0.000      0.967 1.000 0.000
#&gt; GSM38724     1   0.000      0.967 1.000 0.000
#&gt; GSM38725     1   0.000      0.967 1.000 0.000
#&gt; GSM38726     1   0.000      0.967 1.000 0.000
#&gt; GSM38727     1   0.000      0.967 1.000 0.000
#&gt; GSM38728     1   0.000      0.967 1.000 0.000
#&gt; GSM38729     1   0.000      0.967 1.000 0.000
#&gt; GSM38730     1   0.000      0.967 1.000 0.000
#&gt; GSM38731     1   0.000      0.967 1.000 0.000
#&gt; GSM38732     1   0.653      0.831 0.832 0.168
#&gt; GSM38733     1   0.000      0.967 1.000 0.000
#&gt; GSM38734     2   0.000      1.000 0.000 1.000
#&gt; GSM38735     1   0.000      0.967 1.000 0.000
#&gt; GSM38736     2   0.000      1.000 0.000 1.000
#&gt; GSM38737     2   0.000      1.000 0.000 1.000
#&gt; GSM38738     1   0.653      0.831 0.832 0.168
#&gt; GSM38739     1   0.000      0.967 1.000 0.000
#&gt; GSM38740     1   0.000      0.967 1.000 0.000
#&gt; GSM38741     1   0.653      0.831 0.832 0.168
#&gt; GSM38742     2   0.000      1.000 0.000 1.000
#&gt; GSM38743     2   0.000      1.000 0.000 1.000
#&gt; GSM38744     1   0.000      0.967 1.000 0.000
#&gt; GSM38745     1   0.000      0.967 1.000 0.000
#&gt; GSM38746     1   0.000      0.967 1.000 0.000
#&gt; GSM38747     1   0.000      0.967 1.000 0.000
#&gt; GSM38748     2   0.000      1.000 0.000 1.000
#&gt; GSM38749     1   0.000      0.967 1.000 0.000
#&gt; GSM38750     1   0.653      0.831 0.832 0.168
#&gt; GSM38751     1   0.653      0.831 0.832 0.168
#&gt; GSM38752     2   0.000      1.000 0.000 1.000
#&gt; GSM38753     2   0.000      1.000 0.000 1.000
#&gt; GSM38754     2   0.000      1.000 0.000 1.000
#&gt; GSM38755     1   0.653      0.831 0.832 0.168
#&gt; GSM38756     2   0.000      1.000 0.000 1.000
#&gt; GSM38757     1   0.653      0.831 0.832 0.168
#&gt; GSM38758     2   0.000      1.000 0.000 1.000
#&gt; GSM38759     1   0.000      0.967 1.000 0.000
#&gt; GSM38760     1   0.000      0.967 1.000 0.000
#&gt; GSM38761     2   0.000      1.000 0.000 1.000
#&gt; GSM38762     2   0.000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3
#&gt; GSM38712     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38713     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38714     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38715     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38716     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38717     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38718     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38719     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38720     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38721     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38722     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38723     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38724     1  0.0237      0.996 0.996  0 0.004
#&gt; GSM38725     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38726     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38727     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38728     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38729     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38730     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38731     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38732     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM38733     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38734     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38735     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38736     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38738     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM38739     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38740     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38741     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM38742     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38744     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38745     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38746     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38747     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38748     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38749     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38750     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM38751     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM38752     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38753     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38754     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38755     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM38756     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38757     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM38758     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38759     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38760     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM38761     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.2921      0.896 0.860 0.140 0.000 0.000
#&gt; GSM38714     1  0.2921      0.896 0.860 0.140 0.000 0.000
#&gt; GSM38715     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.3311      0.887 0.828 0.172 0.000 0.000
#&gt; GSM38722     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38724     1  0.4088      0.860 0.764 0.232 0.004 0.000
#&gt; GSM38725     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38728     1  0.3907      0.862 0.768 0.232 0.000 0.000
#&gt; GSM38729     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM38733     1  0.3907      0.862 0.768 0.232 0.000 0.000
#&gt; GSM38734     4  0.0000      0.990 0.000 0.000 0.000 1.000
#&gt; GSM38735     1  0.3907      0.862 0.768 0.232 0.000 0.000
#&gt; GSM38736     2  0.3907      0.989 0.000 0.768 0.000 0.232
#&gt; GSM38737     2  0.4040      0.986 0.000 0.752 0.000 0.248
#&gt; GSM38738     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM38739     1  0.3907      0.862 0.768 0.232 0.000 0.000
#&gt; GSM38740     1  0.1637      0.911 0.940 0.060 0.000 0.000
#&gt; GSM38741     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM38742     2  0.4040      0.986 0.000 0.752 0.000 0.248
#&gt; GSM38743     2  0.3907      0.989 0.000 0.768 0.000 0.232
#&gt; GSM38744     1  0.0000      0.916 1.000 0.000 0.000 0.000
#&gt; GSM38745     1  0.1637      0.911 0.940 0.060 0.000 0.000
#&gt; GSM38746     1  0.3356      0.886 0.824 0.176 0.000 0.000
#&gt; GSM38747     1  0.3356      0.886 0.824 0.176 0.000 0.000
#&gt; GSM38748     4  0.0707      0.980 0.000 0.020 0.000 0.980
#&gt; GSM38749     1  0.3907      0.862 0.768 0.232 0.000 0.000
#&gt; GSM38750     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM38751     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM38752     4  0.0000      0.990 0.000 0.000 0.000 1.000
#&gt; GSM38753     4  0.0000      0.990 0.000 0.000 0.000 1.000
#&gt; GSM38754     4  0.0000      0.990 0.000 0.000 0.000 1.000
#&gt; GSM38755     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM38756     4  0.0707      0.980 0.000 0.020 0.000 0.980
#&gt; GSM38757     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM38758     2  0.3907      0.989 0.000 0.768 0.000 0.232
#&gt; GSM38759     1  0.3907      0.862 0.768 0.232 0.000 0.000
#&gt; GSM38760     1  0.3907      0.862 0.768 0.232 0.000 0.000
#&gt; GSM38761     2  0.3907      0.989 0.000 0.768 0.000 0.232
#&gt; GSM38762     2  0.4040      0.986 0.000 0.752 0.000 0.248
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.2516      0.716 0.860 0.000 0.000 0.000 0.140
#&gt; GSM38714     1  0.2516      0.716 0.860 0.000 0.000 0.000 0.140
#&gt; GSM38715     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     5  0.4287      0.646 0.460 0.000 0.000 0.000 0.540
#&gt; GSM38722     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38724     5  0.4321      0.708 0.396 0.000 0.004 0.000 0.600
#&gt; GSM38725     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38728     5  0.4182      0.710 0.400 0.000 0.000 0.000 0.600
#&gt; GSM38729     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.969 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38733     5  0.4182      0.710 0.400 0.000 0.000 0.000 0.600
#&gt; GSM38734     4  0.0000      0.895 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38735     5  0.0000      0.336 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38736     2  0.0000      0.908 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.3305      0.763 0.000 0.776 0.000 0.224 0.000
#&gt; GSM38738     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38739     5  0.4182      0.710 0.400 0.000 0.000 0.000 0.600
#&gt; GSM38740     5  0.3983      0.272 0.340 0.000 0.000 0.000 0.660
#&gt; GSM38741     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38742     2  0.3305      0.763 0.000 0.776 0.000 0.224 0.000
#&gt; GSM38743     2  0.0000      0.908 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.4182      0.175 0.400 0.000 0.000 0.000 0.600
#&gt; GSM38745     5  0.3983      0.272 0.340 0.000 0.000 0.000 0.660
#&gt; GSM38746     5  0.4283      0.652 0.456 0.000 0.000 0.000 0.544
#&gt; GSM38747     5  0.4283      0.652 0.456 0.000 0.000 0.000 0.544
#&gt; GSM38748     4  0.3336      0.764 0.000 0.228 0.000 0.772 0.000
#&gt; GSM38749     5  0.4182      0.710 0.400 0.000 0.000 0.000 0.600
#&gt; GSM38750     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38751     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38752     4  0.0000      0.895 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38753     4  0.0000      0.895 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38754     4  0.0000      0.895 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38755     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38756     4  0.3336      0.764 0.000 0.228 0.000 0.772 0.000
#&gt; GSM38757     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38758     2  0.0000      0.908 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38759     5  0.4182      0.710 0.400 0.000 0.000 0.000 0.600
#&gt; GSM38760     5  0.4182      0.710 0.400 0.000 0.000 0.000 0.600
#&gt; GSM38761     2  0.0000      0.908 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0794      0.899 0.000 0.972 0.000 0.028 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38713     1   0.226      0.732 0.860 0.000 0.000 0.000 0.000 0.140
#&gt; GSM38714     1   0.226      0.732 0.860 0.000 0.000 0.000 0.000 0.140
#&gt; GSM38715     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38716     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38717     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38719     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     6   0.379      0.791 0.416 0.000 0.000 0.000 0.000 0.584
#&gt; GSM38722     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38723     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38724     6   0.376      0.834 0.352 0.000 0.004 0.000 0.000 0.644
#&gt; GSM38725     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38726     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38728     6   0.363      0.837 0.356 0.000 0.000 0.000 0.000 0.644
#&gt; GSM38729     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38730     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1   0.000      0.971 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38733     6   0.374      0.818 0.392 0.000 0.000 0.000 0.000 0.608
#&gt; GSM38734     4   0.000      0.837 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38735     5   0.376      0.294 0.000 0.000 0.000 0.000 0.600 0.400
#&gt; GSM38736     2   0.000      0.919 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2   0.300      0.809 0.000 0.772 0.000 0.000 0.228 0.000
#&gt; GSM38738     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38739     6   0.363      0.837 0.356 0.000 0.000 0.000 0.000 0.644
#&gt; GSM38740     5   0.470      0.760 0.340 0.000 0.000 0.000 0.600 0.060
#&gt; GSM38741     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38742     2   0.300      0.809 0.000 0.772 0.000 0.000 0.228 0.000
#&gt; GSM38743     2   0.000      0.919 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5   0.376      0.693 0.400 0.000 0.000 0.000 0.600 0.000
#&gt; GSM38745     5   0.470      0.760 0.340 0.000 0.000 0.000 0.600 0.060
#&gt; GSM38746     6   0.378      0.797 0.412 0.000 0.000 0.000 0.000 0.588
#&gt; GSM38747     6   0.378      0.797 0.412 0.000 0.000 0.000 0.000 0.588
#&gt; GSM38748     4   0.466      0.601 0.000 0.228 0.000 0.672 0.100 0.000
#&gt; GSM38749     6   0.363      0.837 0.356 0.000 0.000 0.000 0.000 0.644
#&gt; GSM38750     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38751     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38752     4   0.000      0.837 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38753     4   0.489      0.615 0.000 0.000 0.000 0.572 0.072 0.356
#&gt; GSM38754     4   0.000      0.837 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38755     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38756     6   0.725     -0.674 0.000 0.228 0.000 0.316 0.100 0.356
#&gt; GSM38757     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38758     2   0.000      0.919 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38759     6   0.365      0.836 0.360 0.000 0.000 0.000 0.000 0.640
#&gt; GSM38760     6   0.374      0.818 0.392 0.000 0.000 0.000 0.000 0.608
#&gt; GSM38761     2   0.000      0.919 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38762     2   0.079      0.911 0.000 0.968 0.000 0.000 0.032 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n individual(p) k
#> ATC:hclust 51       0.05676 2
#> ATC:hclust 51       0.01414 3
#> ATC:hclust 51       0.00184 4
#> ATC:hclust 47       0.00140 5
#> ATC:hclust 49       0.00048 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.974       0.990         0.4792 0.523   0.523
#> 3 3 0.867           0.831       0.929         0.3015 0.809   0.651
#> 4 4 0.723           0.867       0.867         0.1586 0.805   0.530
#> 5 5 0.865           0.852       0.865         0.0728 0.972   0.889
#> 6 6 0.846           0.818       0.864         0.0450 0.955   0.801
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.987 1.000 0.000
#&gt; GSM38713     1  0.0000      0.987 1.000 0.000
#&gt; GSM38714     1  0.0000      0.987 1.000 0.000
#&gt; GSM38715     1  0.0000      0.987 1.000 0.000
#&gt; GSM38716     1  0.0000      0.987 1.000 0.000
#&gt; GSM38717     1  0.0000      0.987 1.000 0.000
#&gt; GSM38718     1  0.0000      0.987 1.000 0.000
#&gt; GSM38719     1  0.0000      0.987 1.000 0.000
#&gt; GSM38720     1  0.0000      0.987 1.000 0.000
#&gt; GSM38721     1  0.0000      0.987 1.000 0.000
#&gt; GSM38722     1  0.0000      0.987 1.000 0.000
#&gt; GSM38723     1  0.0000      0.987 1.000 0.000
#&gt; GSM38724     1  0.0000      0.987 1.000 0.000
#&gt; GSM38725     1  0.0000      0.987 1.000 0.000
#&gt; GSM38726     1  0.0000      0.987 1.000 0.000
#&gt; GSM38727     1  0.0000      0.987 1.000 0.000
#&gt; GSM38728     1  0.0000      0.987 1.000 0.000
#&gt; GSM38729     1  0.0000      0.987 1.000 0.000
#&gt; GSM38730     1  0.0000      0.987 1.000 0.000
#&gt; GSM38731     1  0.0000      0.987 1.000 0.000
#&gt; GSM38732     2  0.0000      0.991 0.000 1.000
#&gt; GSM38733     1  0.0000      0.987 1.000 0.000
#&gt; GSM38734     2  0.0000      0.991 0.000 1.000
#&gt; GSM38735     1  0.0000      0.987 1.000 0.000
#&gt; GSM38736     2  0.0000      0.991 0.000 1.000
#&gt; GSM38737     2  0.0000      0.991 0.000 1.000
#&gt; GSM38738     2  0.0000      0.991 0.000 1.000
#&gt; GSM38739     1  0.0000      0.987 1.000 0.000
#&gt; GSM38740     1  0.0000      0.987 1.000 0.000
#&gt; GSM38741     2  0.0376      0.989 0.004 0.996
#&gt; GSM38742     2  0.0000      0.991 0.000 1.000
#&gt; GSM38743     2  0.0000      0.991 0.000 1.000
#&gt; GSM38744     1  0.0000      0.987 1.000 0.000
#&gt; GSM38745     1  0.0000      0.987 1.000 0.000
#&gt; GSM38746     1  0.0000      0.987 1.000 0.000
#&gt; GSM38747     1  0.0000      0.987 1.000 0.000
#&gt; GSM38748     2  0.0000      0.991 0.000 1.000
#&gt; GSM38749     1  0.0000      0.987 1.000 0.000
#&gt; GSM38750     2  0.0376      0.989 0.004 0.996
#&gt; GSM38751     2  0.4022      0.916 0.080 0.920
#&gt; GSM38752     2  0.0000      0.991 0.000 1.000
#&gt; GSM38753     2  0.0000      0.991 0.000 1.000
#&gt; GSM38754     2  0.0000      0.991 0.000 1.000
#&gt; GSM38755     1  0.9580      0.374 0.620 0.380
#&gt; GSM38756     2  0.0000      0.991 0.000 1.000
#&gt; GSM38757     2  0.3431      0.934 0.064 0.936
#&gt; GSM38758     2  0.0000      0.991 0.000 1.000
#&gt; GSM38759     1  0.0000      0.987 1.000 0.000
#&gt; GSM38760     1  0.0000      0.987 1.000 0.000
#&gt; GSM38761     2  0.0000      0.991 0.000 1.000
#&gt; GSM38762     2  0.0000      0.991 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38713     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38714     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38715     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38716     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38717     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38718     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38719     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38720     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38721     1   0.568      0.532 0.684 0.000 0.316
#&gt; GSM38722     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38723     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38724     3   0.271      0.834 0.088 0.000 0.912
#&gt; GSM38725     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38726     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38727     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38728     3   0.271      0.834 0.088 0.000 0.912
#&gt; GSM38729     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38730     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38731     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38732     3   0.116      0.862 0.000 0.028 0.972
#&gt; GSM38733     1   0.597      0.432 0.636 0.000 0.364
#&gt; GSM38734     2   0.207      0.959 0.000 0.940 0.060
#&gt; GSM38735     3   0.603      0.327 0.376 0.000 0.624
#&gt; GSM38736     2   0.000      0.979 0.000 1.000 0.000
#&gt; GSM38737     2   0.000      0.979 0.000 1.000 0.000
#&gt; GSM38738     3   0.116      0.862 0.000 0.028 0.972
#&gt; GSM38739     1   0.615      0.316 0.592 0.000 0.408
#&gt; GSM38740     1   0.116      0.880 0.972 0.000 0.028
#&gt; GSM38741     3   0.116      0.862 0.000 0.028 0.972
#&gt; GSM38742     2   0.000      0.979 0.000 1.000 0.000
#&gt; GSM38743     2   0.000      0.979 0.000 1.000 0.000
#&gt; GSM38744     1   0.116      0.880 0.972 0.000 0.028
#&gt; GSM38745     1   0.116      0.880 0.972 0.000 0.028
#&gt; GSM38746     3   0.626      0.120 0.448 0.000 0.552
#&gt; GSM38747     1   0.000      0.899 1.000 0.000 0.000
#&gt; GSM38748     2   0.129      0.970 0.000 0.968 0.032
#&gt; GSM38749     1   0.565      0.539 0.688 0.000 0.312
#&gt; GSM38750     3   0.116      0.862 0.000 0.028 0.972
#&gt; GSM38751     3   0.116      0.862 0.000 0.028 0.972
#&gt; GSM38752     2   0.207      0.959 0.000 0.940 0.060
#&gt; GSM38753     2   0.207      0.959 0.000 0.940 0.060
#&gt; GSM38754     2   0.207      0.959 0.000 0.940 0.060
#&gt; GSM38755     3   0.116      0.857 0.028 0.000 0.972
#&gt; GSM38756     2   0.000      0.979 0.000 1.000 0.000
#&gt; GSM38757     3   0.116      0.862 0.000 0.028 0.972
#&gt; GSM38758     2   0.000      0.979 0.000 1.000 0.000
#&gt; GSM38759     1   0.553      0.564 0.704 0.000 0.296
#&gt; GSM38760     1   0.608      0.372 0.612 0.000 0.388
#&gt; GSM38761     2   0.000      0.979 0.000 1.000 0.000
#&gt; GSM38762     2   0.000      0.979 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38713     2  0.4981      0.516 0.464 0.536 0.000 0.000
#&gt; GSM38714     2  0.4746      0.680 0.368 0.632 0.000 0.000
#&gt; GSM38715     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38721     2  0.6364      0.796 0.204 0.652 0.144 0.000
#&gt; GSM38722     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38724     2  0.4819      0.541 0.004 0.652 0.344 0.000
#&gt; GSM38725     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38728     2  0.4781      0.553 0.004 0.660 0.336 0.000
#&gt; GSM38729     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.988 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.0000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38733     2  0.6295      0.796 0.196 0.660 0.144 0.000
#&gt; GSM38734     4  0.5142      0.867 0.000 0.192 0.064 0.744
#&gt; GSM38735     2  0.4487      0.725 0.092 0.808 0.100 0.000
#&gt; GSM38736     4  0.0000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM38737     4  0.1059      0.900 0.000 0.016 0.012 0.972
#&gt; GSM38738     3  0.0000      0.969 0.000 0.000 1.000 0.000
#&gt; GSM38739     2  0.6121      0.785 0.156 0.680 0.164 0.000
#&gt; GSM38740     2  0.4888      0.460 0.412 0.588 0.000 0.000
#&gt; GSM38741     3  0.0592      0.978 0.000 0.016 0.984 0.000
#&gt; GSM38742     4  0.1059      0.900 0.000 0.016 0.012 0.972
#&gt; GSM38743     4  0.0000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM38744     1  0.3123      0.798 0.844 0.156 0.000 0.000
#&gt; GSM38745     2  0.4431      0.660 0.304 0.696 0.000 0.000
#&gt; GSM38746     2  0.5716      0.686 0.068 0.680 0.252 0.000
#&gt; GSM38747     2  0.4697      0.690 0.356 0.644 0.000 0.000
#&gt; GSM38748     4  0.4149      0.881 0.000 0.168 0.028 0.804
#&gt; GSM38749     2  0.6104      0.799 0.180 0.680 0.140 0.000
#&gt; GSM38750     3  0.0592      0.978 0.000 0.016 0.984 0.000
#&gt; GSM38751     3  0.1302      0.967 0.000 0.044 0.956 0.000
#&gt; GSM38752     4  0.5212      0.866 0.000 0.192 0.068 0.740
#&gt; GSM38753     4  0.5212      0.866 0.000 0.192 0.068 0.740
#&gt; GSM38754     4  0.5212      0.866 0.000 0.192 0.068 0.740
#&gt; GSM38755     3  0.1302      0.967 0.000 0.044 0.956 0.000
#&gt; GSM38756     4  0.3591      0.885 0.000 0.168 0.008 0.824
#&gt; GSM38757     3  0.1022      0.975 0.000 0.032 0.968 0.000
#&gt; GSM38758     4  0.0000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM38759     2  0.6058      0.799 0.180 0.684 0.136 0.000
#&gt; GSM38760     2  0.6110      0.797 0.176 0.680 0.144 0.000
#&gt; GSM38761     4  0.0000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM38762     4  0.0000      0.901 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0290      0.961 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38713     4  0.3906      0.715 0.132 0.000 0.000 0.800 0.068
#&gt; GSM38714     4  0.3390      0.780 0.100 0.000 0.000 0.840 0.060
#&gt; GSM38715     1  0.0290      0.961 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38716     1  0.0290      0.961 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38717     1  0.0510      0.956 0.984 0.000 0.000 0.000 0.016
#&gt; GSM38718     1  0.0510      0.956 0.984 0.000 0.000 0.000 0.016
#&gt; GSM38719     1  0.0290      0.961 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38720     1  0.0290      0.961 0.992 0.000 0.000 0.000 0.008
#&gt; GSM38721     4  0.2790      0.828 0.068 0.000 0.000 0.880 0.052
#&gt; GSM38722     1  0.0162      0.962 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38723     1  0.0162      0.962 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38724     4  0.1877      0.788 0.000 0.000 0.064 0.924 0.012
#&gt; GSM38725     1  0.0162      0.962 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38726     1  0.0162      0.962 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38727     1  0.0162      0.962 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38728     4  0.1877      0.788 0.000 0.000 0.064 0.924 0.012
#&gt; GSM38729     1  0.0000      0.962 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0162      0.962 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38731     1  0.0162      0.962 0.996 0.000 0.000 0.000 0.004
#&gt; GSM38732     3  0.1281      0.988 0.000 0.000 0.956 0.032 0.012
#&gt; GSM38733     4  0.1877      0.837 0.064 0.000 0.000 0.924 0.012
#&gt; GSM38734     2  0.4597      0.758 0.000 0.564 0.012 0.000 0.424
#&gt; GSM38735     5  0.5646      0.609 0.028 0.000 0.028 0.460 0.484
#&gt; GSM38736     2  0.0000      0.799 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.1787      0.797 0.000 0.940 0.012 0.016 0.032
#&gt; GSM38738     3  0.1281      0.988 0.000 0.000 0.956 0.032 0.012
#&gt; GSM38739     4  0.2868      0.808 0.032 0.000 0.012 0.884 0.072
#&gt; GSM38740     5  0.6633      0.746 0.168 0.000 0.020 0.276 0.536
#&gt; GSM38741     3  0.1124      0.994 0.000 0.000 0.960 0.036 0.004
#&gt; GSM38742     2  0.1787      0.797 0.000 0.940 0.012 0.016 0.032
#&gt; GSM38743     2  0.0000      0.799 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     1  0.5430      0.220 0.576 0.000 0.020 0.032 0.372
#&gt; GSM38745     5  0.6333      0.787 0.108 0.000 0.020 0.336 0.536
#&gt; GSM38746     4  0.3592      0.776 0.016 0.000 0.028 0.832 0.124
#&gt; GSM38747     4  0.4083      0.743 0.080 0.000 0.000 0.788 0.132
#&gt; GSM38748     2  0.4517      0.766 0.000 0.616 0.004 0.008 0.372
#&gt; GSM38749     4  0.2830      0.813 0.044 0.000 0.000 0.876 0.080
#&gt; GSM38750     3  0.1124      0.994 0.000 0.000 0.960 0.036 0.004
#&gt; GSM38751     3  0.1124      0.994 0.000 0.000 0.960 0.036 0.004
#&gt; GSM38752     2  0.4597      0.758 0.000 0.564 0.012 0.000 0.424
#&gt; GSM38753     2  0.4936      0.757 0.000 0.564 0.012 0.012 0.412
#&gt; GSM38754     2  0.4597      0.758 0.000 0.564 0.012 0.000 0.424
#&gt; GSM38755     3  0.0963      0.994 0.000 0.000 0.964 0.036 0.000
#&gt; GSM38756     2  0.4675      0.767 0.000 0.620 0.004 0.016 0.360
#&gt; GSM38757     3  0.0963      0.994 0.000 0.000 0.964 0.036 0.000
#&gt; GSM38758     2  0.0162      0.800 0.000 0.996 0.000 0.004 0.000
#&gt; GSM38759     4  0.1597      0.843 0.048 0.000 0.000 0.940 0.012
#&gt; GSM38760     4  0.1522      0.843 0.044 0.000 0.000 0.944 0.012
#&gt; GSM38761     2  0.0162      0.800 0.000 0.996 0.000 0.004 0.000
#&gt; GSM38762     2  0.0798      0.798 0.000 0.976 0.008 0.016 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.0363      0.918 0.988 0.000 0.000 0.012 0.000 0.000
#&gt; GSM38713     6  0.5124      0.638 0.056 0.000 0.000 0.232 0.048 0.664
#&gt; GSM38714     6  0.4696      0.685 0.036 0.000 0.000 0.212 0.048 0.704
#&gt; GSM38715     1  0.2941      0.801 0.780 0.000 0.000 0.220 0.000 0.000
#&gt; GSM38716     1  0.1863      0.878 0.896 0.000 0.000 0.104 0.000 0.000
#&gt; GSM38717     1  0.3073      0.808 0.788 0.000 0.000 0.204 0.008 0.000
#&gt; GSM38718     1  0.3073      0.808 0.788 0.000 0.000 0.204 0.008 0.000
#&gt; GSM38719     1  0.0260      0.919 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM38720     1  0.2562      0.838 0.828 0.000 0.000 0.172 0.000 0.000
#&gt; GSM38721     6  0.2934      0.836 0.024 0.000 0.000 0.064 0.044 0.868
#&gt; GSM38722     1  0.0363      0.921 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM38723     1  0.0363      0.921 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM38724     6  0.1492      0.833 0.000 0.000 0.024 0.036 0.000 0.940
#&gt; GSM38725     1  0.0363      0.921 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM38726     1  0.0363      0.921 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM38727     1  0.0363      0.921 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM38728     6  0.1552      0.834 0.004 0.000 0.020 0.036 0.000 0.940
#&gt; GSM38729     1  0.0717      0.919 0.976 0.000 0.000 0.016 0.008 0.000
#&gt; GSM38730     1  0.0363      0.921 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM38731     1  0.0363      0.921 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM38732     3  0.0291      0.992 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; GSM38733     6  0.1492      0.840 0.024 0.000 0.000 0.036 0.000 0.940
#&gt; GSM38734     4  0.3756      0.925 0.000 0.400 0.000 0.600 0.000 0.000
#&gt; GSM38735     5  0.3777      0.718 0.008 0.000 0.000 0.028 0.756 0.208
#&gt; GSM38736     2  0.0000      0.802 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.2328      0.762 0.000 0.904 0.000 0.044 0.032 0.020
#&gt; GSM38738     3  0.0291      0.992 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; GSM38739     6  0.3972      0.809 0.020 0.000 0.004 0.108 0.072 0.796
#&gt; GSM38740     5  0.2867      0.791 0.040 0.000 0.000 0.000 0.848 0.112
#&gt; GSM38741     3  0.0291      0.994 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; GSM38742     2  0.2328      0.762 0.000 0.904 0.000 0.044 0.032 0.020
#&gt; GSM38743     2  0.0000      0.802 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.3592      0.457 0.344 0.000 0.000 0.000 0.656 0.000
#&gt; GSM38745     5  0.2815      0.787 0.032 0.000 0.000 0.000 0.848 0.120
#&gt; GSM38746     6  0.4250      0.798 0.008 0.000 0.016 0.108 0.092 0.776
#&gt; GSM38747     6  0.4505      0.789 0.028 0.000 0.000 0.120 0.104 0.748
#&gt; GSM38748     4  0.4992      0.793 0.000 0.460 0.000 0.472 0.068 0.000
#&gt; GSM38749     6  0.3965      0.810 0.024 0.000 0.000 0.108 0.076 0.792
#&gt; GSM38750     3  0.0291      0.994 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; GSM38751     3  0.0291      0.994 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; GSM38752     4  0.3756      0.925 0.000 0.400 0.000 0.600 0.000 0.000
#&gt; GSM38753     4  0.5160      0.880 0.000 0.400 0.000 0.520 0.076 0.004
#&gt; GSM38754     4  0.3756      0.925 0.000 0.400 0.000 0.600 0.000 0.000
#&gt; GSM38755     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38756     2  0.5454     -0.823 0.000 0.460 0.000 0.432 0.104 0.004
#&gt; GSM38757     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38758     2  0.0865      0.788 0.000 0.964 0.000 0.000 0.036 0.000
#&gt; GSM38759     6  0.0632      0.846 0.024 0.000 0.000 0.000 0.000 0.976
#&gt; GSM38760     6  0.1564      0.844 0.024 0.000 0.000 0.040 0.000 0.936
#&gt; GSM38761     2  0.0865      0.788 0.000 0.964 0.000 0.000 0.036 0.000
#&gt; GSM38762     2  0.1257      0.792 0.000 0.952 0.000 0.000 0.028 0.020
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n individual(p) k
#> ATC:kmeans 50       0.02722 2
#> ATC:kmeans 46       0.03187 3
#> ATC:kmeans 50       0.03170 4
#> ATC:kmeans 50       0.01339 5
#> ATC:kmeans 49       0.00169 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.991       0.996         0.4884 0.514   0.514
#> 3 3 0.918           0.936       0.956         0.1724 0.900   0.807
#> 4 4 0.802           0.901       0.929         0.1754 0.874   0.702
#> 5 5 0.778           0.810       0.890         0.0584 0.982   0.940
#> 6 6 0.809           0.843       0.915         0.0498 0.947   0.818
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.993 1.000 0.000
#&gt; GSM38713     1   0.000      0.993 1.000 0.000
#&gt; GSM38714     1   0.000      0.993 1.000 0.000
#&gt; GSM38715     1   0.000      0.993 1.000 0.000
#&gt; GSM38716     1   0.000      0.993 1.000 0.000
#&gt; GSM38717     1   0.000      0.993 1.000 0.000
#&gt; GSM38718     1   0.000      0.993 1.000 0.000
#&gt; GSM38719     1   0.000      0.993 1.000 0.000
#&gt; GSM38720     1   0.000      0.993 1.000 0.000
#&gt; GSM38721     1   0.000      0.993 1.000 0.000
#&gt; GSM38722     1   0.000      0.993 1.000 0.000
#&gt; GSM38723     1   0.000      0.993 1.000 0.000
#&gt; GSM38724     1   0.738      0.737 0.792 0.208
#&gt; GSM38725     1   0.000      0.993 1.000 0.000
#&gt; GSM38726     1   0.000      0.993 1.000 0.000
#&gt; GSM38727     1   0.000      0.993 1.000 0.000
#&gt; GSM38728     1   0.000      0.993 1.000 0.000
#&gt; GSM38729     1   0.000      0.993 1.000 0.000
#&gt; GSM38730     1   0.000      0.993 1.000 0.000
#&gt; GSM38731     1   0.000      0.993 1.000 0.000
#&gt; GSM38732     2   0.000      1.000 0.000 1.000
#&gt; GSM38733     1   0.000      0.993 1.000 0.000
#&gt; GSM38734     2   0.000      1.000 0.000 1.000
#&gt; GSM38735     1   0.000      0.993 1.000 0.000
#&gt; GSM38736     2   0.000      1.000 0.000 1.000
#&gt; GSM38737     2   0.000      1.000 0.000 1.000
#&gt; GSM38738     2   0.000      1.000 0.000 1.000
#&gt; GSM38739     1   0.000      0.993 1.000 0.000
#&gt; GSM38740     1   0.000      0.993 1.000 0.000
#&gt; GSM38741     2   0.000      1.000 0.000 1.000
#&gt; GSM38742     2   0.000      1.000 0.000 1.000
#&gt; GSM38743     2   0.000      1.000 0.000 1.000
#&gt; GSM38744     1   0.000      0.993 1.000 0.000
#&gt; GSM38745     1   0.000      0.993 1.000 0.000
#&gt; GSM38746     1   0.000      0.993 1.000 0.000
#&gt; GSM38747     1   0.000      0.993 1.000 0.000
#&gt; GSM38748     2   0.000      1.000 0.000 1.000
#&gt; GSM38749     1   0.000      0.993 1.000 0.000
#&gt; GSM38750     2   0.000      1.000 0.000 1.000
#&gt; GSM38751     2   0.000      1.000 0.000 1.000
#&gt; GSM38752     2   0.000      1.000 0.000 1.000
#&gt; GSM38753     2   0.000      1.000 0.000 1.000
#&gt; GSM38754     2   0.000      1.000 0.000 1.000
#&gt; GSM38755     2   0.000      1.000 0.000 1.000
#&gt; GSM38756     2   0.000      1.000 0.000 1.000
#&gt; GSM38757     2   0.000      1.000 0.000 1.000
#&gt; GSM38758     2   0.000      1.000 0.000 1.000
#&gt; GSM38759     1   0.000      0.993 1.000 0.000
#&gt; GSM38760     1   0.000      0.993 1.000 0.000
#&gt; GSM38761     2   0.000      1.000 0.000 1.000
#&gt; GSM38762     2   0.000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38721     1  0.0892      0.967 0.980 0.000 0.020
#&gt; GSM38722     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38724     3  0.6180      0.271 0.416 0.000 0.584
#&gt; GSM38725     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38728     1  0.1163      0.962 0.972 0.000 0.028
#&gt; GSM38729     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38732     3  0.5216      0.718 0.000 0.260 0.740
#&gt; GSM38733     1  0.1163      0.962 0.972 0.000 0.028
#&gt; GSM38734     2  0.4235      0.744 0.000 0.824 0.176
#&gt; GSM38735     1  0.2711      0.930 0.912 0.000 0.088
#&gt; GSM38736     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38738     3  0.3267      0.877 0.000 0.116 0.884
#&gt; GSM38739     1  0.2537      0.935 0.920 0.000 0.080
#&gt; GSM38740     1  0.2625      0.933 0.916 0.000 0.084
#&gt; GSM38741     3  0.3619      0.863 0.000 0.136 0.864
#&gt; GSM38742     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38744     1  0.2625      0.933 0.916 0.000 0.084
#&gt; GSM38745     1  0.2711      0.930 0.912 0.000 0.088
#&gt; GSM38746     1  0.2537      0.935 0.920 0.000 0.080
#&gt; GSM38747     1  0.0237      0.976 0.996 0.000 0.004
#&gt; GSM38748     2  0.0237      0.978 0.000 0.996 0.004
#&gt; GSM38749     1  0.2448      0.938 0.924 0.000 0.076
#&gt; GSM38750     3  0.3267      0.877 0.000 0.116 0.884
#&gt; GSM38751     3  0.3267      0.877 0.000 0.116 0.884
#&gt; GSM38752     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38753     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38754     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38755     3  0.3267      0.877 0.000 0.116 0.884
#&gt; GSM38756     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38757     3  0.3267      0.877 0.000 0.116 0.884
#&gt; GSM38758     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38759     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38760     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM38761     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000      0.982 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.2466      0.844 0.900 0.096 0.004 0.000
#&gt; GSM38722     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.6903      0.433 0.224 0.184 0.592 0.000
#&gt; GSM38725     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38728     1  0.3626      0.718 0.812 0.184 0.004 0.000
#&gt; GSM38729     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.961 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.3444      0.754 0.000 0.000 0.816 0.184
#&gt; GSM38733     1  0.3626      0.718 0.812 0.184 0.004 0.000
#&gt; GSM38734     4  0.3311      0.773 0.000 0.000 0.172 0.828
#&gt; GSM38735     2  0.3444      0.776 0.184 0.816 0.000 0.000
#&gt; GSM38736     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38737     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38738     3  0.0336      0.896 0.000 0.000 0.992 0.008
#&gt; GSM38739     2  0.4978      0.800 0.384 0.612 0.004 0.000
#&gt; GSM38740     2  0.4222      0.850 0.272 0.728 0.000 0.000
#&gt; GSM38741     3  0.1867      0.861 0.000 0.000 0.928 0.072
#&gt; GSM38742     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38743     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38744     2  0.4222      0.850 0.272 0.728 0.000 0.000
#&gt; GSM38745     2  0.3528      0.787 0.192 0.808 0.000 0.000
#&gt; GSM38746     2  0.4950      0.806 0.376 0.620 0.004 0.000
#&gt; GSM38747     1  0.1576      0.899 0.948 0.048 0.004 0.000
#&gt; GSM38748     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38749     2  0.5168      0.594 0.492 0.504 0.004 0.000
#&gt; GSM38750     3  0.0336      0.896 0.000 0.000 0.992 0.008
#&gt; GSM38751     3  0.0469      0.896 0.000 0.000 0.988 0.012
#&gt; GSM38752     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38753     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38754     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38755     3  0.0188      0.894 0.000 0.000 0.996 0.004
#&gt; GSM38756     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38757     3  0.0336      0.896 0.000 0.000 0.992 0.008
#&gt; GSM38758     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38759     1  0.1118      0.922 0.964 0.036 0.000 0.000
#&gt; GSM38760     1  0.0336      0.953 0.992 0.008 0.000 0.000
#&gt; GSM38761     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM38762     4  0.0000      0.984 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     1  0.4558      0.547 0.724 0.000 0.000 0.060 0.216
#&gt; GSM38722     1  0.0162      0.900 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38723     1  0.0162      0.900 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38724     5  0.6626     -0.226 0.000 0.000 0.340 0.228 0.432
#&gt; GSM38725     1  0.0162      0.900 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38726     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0162      0.900 0.996 0.000 0.000 0.004 0.000
#&gt; GSM38728     1  0.5591      0.073 0.496 0.000 0.000 0.072 0.432
#&gt; GSM38729     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.902 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.2891      0.748 0.000 0.176 0.824 0.000 0.000
#&gt; GSM38733     1  0.4109      0.501 0.700 0.000 0.000 0.012 0.288
#&gt; GSM38734     2  0.3123      0.791 0.000 0.828 0.160 0.000 0.012
#&gt; GSM38735     5  0.4268      0.433 0.000 0.000 0.000 0.444 0.556
#&gt; GSM38736     2  0.0000      0.980 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.980 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38738     3  0.0404      0.918 0.000 0.012 0.988 0.000 0.000
#&gt; GSM38739     4  0.3957      0.896 0.280 0.000 0.000 0.712 0.008
#&gt; GSM38740     5  0.6282      0.397 0.216 0.000 0.000 0.248 0.536
#&gt; GSM38741     3  0.2674      0.811 0.000 0.120 0.868 0.000 0.012
#&gt; GSM38742     2  0.0000      0.980 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000      0.980 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.6278      0.403 0.212 0.000 0.000 0.252 0.536
#&gt; GSM38745     5  0.5781      0.460 0.104 0.000 0.000 0.344 0.552
#&gt; GSM38746     4  0.3395      0.839 0.236 0.000 0.000 0.764 0.000
#&gt; GSM38747     1  0.3561      0.486 0.740 0.000 0.000 0.260 0.000
#&gt; GSM38748     2  0.0693      0.973 0.000 0.980 0.008 0.000 0.012
#&gt; GSM38749     4  0.4029      0.865 0.316 0.000 0.000 0.680 0.004
#&gt; GSM38750     3  0.0000      0.922 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38751     3  0.0000      0.922 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38752     2  0.0404      0.978 0.000 0.988 0.000 0.000 0.012
#&gt; GSM38753     2  0.0404      0.978 0.000 0.988 0.000 0.000 0.012
#&gt; GSM38754     2  0.0404      0.978 0.000 0.988 0.000 0.000 0.012
#&gt; GSM38755     3  0.0000      0.922 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38756     2  0.0404      0.978 0.000 0.988 0.000 0.000 0.012
#&gt; GSM38757     3  0.0000      0.922 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38758     2  0.0000      0.980 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38759     1  0.1914      0.824 0.924 0.000 0.000 0.016 0.060
#&gt; GSM38760     1  0.2230      0.760 0.884 0.000 0.000 0.116 0.000
#&gt; GSM38761     2  0.0000      0.980 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      0.980 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.0146      0.939 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM38713     1  0.0458      0.933 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM38714     1  0.0458      0.933 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM38715     1  0.0458      0.933 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM38716     1  0.0291      0.939 0.992 0.000 0.000 0.004 0.000 0.004
#&gt; GSM38717     1  0.0260      0.937 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM38718     1  0.0260      0.937 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM38719     1  0.0146      0.938 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM38720     1  0.0146      0.938 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM38721     6  0.3804      0.469 0.424 0.000 0.000 0.000 0.000 0.576
#&gt; GSM38722     1  0.0458      0.935 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; GSM38723     1  0.0632      0.929 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; GSM38724     6  0.4554      0.228 0.000 0.000 0.160 0.124 0.004 0.712
#&gt; GSM38725     1  0.0363      0.937 0.988 0.000 0.000 0.012 0.000 0.000
#&gt; GSM38726     1  0.0260      0.939 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM38727     1  0.0547      0.932 0.980 0.000 0.000 0.020 0.000 0.000
#&gt; GSM38728     6  0.1814      0.451 0.100 0.000 0.000 0.000 0.000 0.900
#&gt; GSM38729     1  0.0000      0.939 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0260      0.939 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM38731     1  0.0260      0.939 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM38732     3  0.3593      0.711 0.000 0.176 0.788 0.024 0.004 0.008
#&gt; GSM38733     6  0.3765      0.505 0.404 0.000 0.000 0.000 0.000 0.596
#&gt; GSM38734     2  0.4303      0.748 0.000 0.752 0.176 0.028 0.004 0.040
#&gt; GSM38735     5  0.0363      0.617 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; GSM38736     2  0.0000      0.947 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      0.947 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38738     3  0.0260      0.888 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM38739     4  0.1983      0.968 0.072 0.000 0.000 0.908 0.020 0.000
#&gt; GSM38740     5  0.3896      0.780 0.196 0.000 0.000 0.056 0.748 0.000
#&gt; GSM38741     3  0.4305      0.661 0.000 0.196 0.740 0.020 0.004 0.040
#&gt; GSM38742     2  0.0000      0.947 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000      0.947 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.3896      0.780 0.196 0.000 0.000 0.056 0.748 0.000
#&gt; GSM38745     5  0.3159      0.772 0.100 0.000 0.000 0.068 0.832 0.000
#&gt; GSM38746     4  0.2711      0.953 0.080 0.000 0.000 0.876 0.020 0.024
#&gt; GSM38747     1  0.4328      0.435 0.672 0.000 0.000 0.284 0.004 0.040
#&gt; GSM38748     2  0.2763      0.910 0.000 0.884 0.048 0.024 0.004 0.040
#&gt; GSM38749     4  0.2094      0.968 0.080 0.000 0.000 0.900 0.020 0.000
#&gt; GSM38750     3  0.0000      0.889 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38751     3  0.0291      0.888 0.000 0.004 0.992 0.000 0.000 0.004
#&gt; GSM38752     2  0.2409      0.925 0.000 0.904 0.024 0.028 0.004 0.040
#&gt; GSM38753     2  0.1788      0.934 0.000 0.928 0.000 0.028 0.004 0.040
#&gt; GSM38754     2  0.2326      0.927 0.000 0.908 0.020 0.028 0.004 0.040
#&gt; GSM38755     3  0.0405      0.885 0.000 0.000 0.988 0.000 0.004 0.008
#&gt; GSM38756     2  0.1708      0.935 0.000 0.932 0.000 0.024 0.004 0.040
#&gt; GSM38757     3  0.0146      0.888 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM38758     2  0.0000      0.947 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38759     1  0.2318      0.842 0.904 0.000 0.000 0.020 0.048 0.028
#&gt; GSM38760     1  0.3555      0.518 0.712 0.000 0.000 0.280 0.000 0.008
#&gt; GSM38761     2  0.0000      0.947 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      0.947 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n individual(p) k
#> ATC:skmeans 51       0.02014 2
#> ATC:skmeans 50       0.01908 3
#> ATC:skmeans 50       0.00209 4
#> ATC:skmeans 44       0.00635 5
#> ATC:skmeans 47       0.00466 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.991       0.996         0.4790 0.523   0.523
#> 3 3 0.821           0.938       0.968         0.3736 0.706   0.488
#> 4 4 0.838           0.905       0.919         0.0801 0.967   0.901
#> 5 5 0.873           0.783       0.889         0.0783 0.882   0.631
#> 6 6 0.868           0.813       0.901         0.0569 0.953   0.794
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1  0.0000      0.994 1.000 0.000
#&gt; GSM38713     1  0.0000      0.994 1.000 0.000
#&gt; GSM38714     1  0.0000      0.994 1.000 0.000
#&gt; GSM38715     1  0.0000      0.994 1.000 0.000
#&gt; GSM38716     1  0.0000      0.994 1.000 0.000
#&gt; GSM38717     1  0.0000      0.994 1.000 0.000
#&gt; GSM38718     1  0.0000      0.994 1.000 0.000
#&gt; GSM38719     1  0.0000      0.994 1.000 0.000
#&gt; GSM38720     1  0.0000      0.994 1.000 0.000
#&gt; GSM38721     1  0.0000      0.994 1.000 0.000
#&gt; GSM38722     1  0.0000      0.994 1.000 0.000
#&gt; GSM38723     1  0.0000      0.994 1.000 0.000
#&gt; GSM38724     1  0.0000      0.994 1.000 0.000
#&gt; GSM38725     1  0.0000      0.994 1.000 0.000
#&gt; GSM38726     1  0.0000      0.994 1.000 0.000
#&gt; GSM38727     1  0.0000      0.994 1.000 0.000
#&gt; GSM38728     1  0.0000      0.994 1.000 0.000
#&gt; GSM38729     1  0.0000      0.994 1.000 0.000
#&gt; GSM38730     1  0.0000      0.994 1.000 0.000
#&gt; GSM38731     1  0.0000      0.994 1.000 0.000
#&gt; GSM38732     2  0.0376      0.997 0.004 0.996
#&gt; GSM38733     1  0.0000      0.994 1.000 0.000
#&gt; GSM38734     2  0.0000      0.999 0.000 1.000
#&gt; GSM38735     1  0.0000      0.994 1.000 0.000
#&gt; GSM38736     2  0.0000      0.999 0.000 1.000
#&gt; GSM38737     2  0.0000      0.999 0.000 1.000
#&gt; GSM38738     2  0.0376      0.997 0.004 0.996
#&gt; GSM38739     1  0.0000      0.994 1.000 0.000
#&gt; GSM38740     1  0.0000      0.994 1.000 0.000
#&gt; GSM38741     2  0.0376      0.997 0.004 0.996
#&gt; GSM38742     2  0.0000      0.999 0.000 1.000
#&gt; GSM38743     2  0.0000      0.999 0.000 1.000
#&gt; GSM38744     1  0.0000      0.994 1.000 0.000
#&gt; GSM38745     1  0.0000      0.994 1.000 0.000
#&gt; GSM38746     1  0.0000      0.994 1.000 0.000
#&gt; GSM38747     1  0.0000      0.994 1.000 0.000
#&gt; GSM38748     2  0.0000      0.999 0.000 1.000
#&gt; GSM38749     1  0.0000      0.994 1.000 0.000
#&gt; GSM38750     2  0.0376      0.997 0.004 0.996
#&gt; GSM38751     2  0.0376      0.997 0.004 0.996
#&gt; GSM38752     2  0.0000      0.999 0.000 1.000
#&gt; GSM38753     2  0.0000      0.999 0.000 1.000
#&gt; GSM38754     2  0.0000      0.999 0.000 1.000
#&gt; GSM38755     1  0.7139      0.756 0.804 0.196
#&gt; GSM38756     2  0.0000      0.999 0.000 1.000
#&gt; GSM38757     2  0.0376      0.997 0.004 0.996
#&gt; GSM38758     2  0.0000      0.999 0.000 1.000
#&gt; GSM38759     1  0.0000      0.994 1.000 0.000
#&gt; GSM38760     1  0.0000      0.994 1.000 0.000
#&gt; GSM38761     2  0.0000      0.999 0.000 1.000
#&gt; GSM38762     2  0.0000      0.999 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38713     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38714     1   0.175      0.938 0.952 0.000 0.048
#&gt; GSM38715     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38716     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38717     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38718     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38719     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38720     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38721     3   0.382      0.887 0.148 0.000 0.852
#&gt; GSM38722     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38723     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38724     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38725     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38726     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38727     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38728     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38729     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38730     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38731     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38732     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38733     3   0.382      0.887 0.148 0.000 0.852
#&gt; GSM38734     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38735     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38736     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38737     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38738     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38739     3   0.382      0.887 0.148 0.000 0.852
#&gt; GSM38740     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38741     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38742     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38743     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38744     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38745     1   0.000      0.987 1.000 0.000 0.000
#&gt; GSM38746     3   0.382      0.887 0.148 0.000 0.852
#&gt; GSM38747     1   0.440      0.739 0.812 0.000 0.188
#&gt; GSM38748     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38749     3   0.382      0.887 0.148 0.000 0.852
#&gt; GSM38750     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38751     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38752     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38753     2   0.603      0.363 0.000 0.624 0.376
#&gt; GSM38754     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38755     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38756     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38757     3   0.000      0.922 0.000 0.000 1.000
#&gt; GSM38758     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38759     3   0.382      0.887 0.148 0.000 0.852
#&gt; GSM38760     3   0.375      0.888 0.144 0.000 0.856
#&gt; GSM38761     2   0.000      0.967 0.000 1.000 0.000
#&gt; GSM38762     2   0.000      0.967 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38713     1   0.102      0.924 0.968 0.000 0.032 0.000
#&gt; GSM38714     1   0.361      0.785 0.800 0.000 0.200 0.000
#&gt; GSM38715     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38716     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38717     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38718     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38719     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38720     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38721     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38722     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38723     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38724     3   0.253      0.891 0.000 0.112 0.888 0.000
#&gt; GSM38725     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38726     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38727     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38728     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38729     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38730     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38731     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38732     3   0.555      0.809 0.000 0.200 0.716 0.084
#&gt; GSM38733     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38734     4   0.000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM38735     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38736     2   0.361      1.000 0.000 0.800 0.000 0.200
#&gt; GSM38737     2   0.361      1.000 0.000 0.800 0.000 0.200
#&gt; GSM38738     3   0.361      0.873 0.000 0.200 0.800 0.000
#&gt; GSM38739     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38740     1   0.361      0.785 0.800 0.000 0.200 0.000
#&gt; GSM38741     3   0.361      0.873 0.000 0.200 0.800 0.000
#&gt; GSM38742     2   0.361      1.000 0.000 0.800 0.000 0.200
#&gt; GSM38743     2   0.361      1.000 0.000 0.800 0.000 0.200
#&gt; GSM38744     1   0.000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM38745     1   0.361      0.785 0.800 0.000 0.200 0.000
#&gt; GSM38746     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38747     1   0.482      0.494 0.612 0.000 0.388 0.000
#&gt; GSM38748     4   0.265      0.831 0.000 0.120 0.000 0.880
#&gt; GSM38749     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38750     3   0.361      0.873 0.000 0.200 0.800 0.000
#&gt; GSM38751     3   0.361      0.873 0.000 0.200 0.800 0.000
#&gt; GSM38752     4   0.000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM38753     4   0.000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM38754     4   0.000      0.917 0.000 0.000 0.000 1.000
#&gt; GSM38755     3   0.361      0.873 0.000 0.200 0.800 0.000
#&gt; GSM38756     4   0.361      0.715 0.000 0.200 0.000 0.800
#&gt; GSM38757     3   0.361      0.873 0.000 0.200 0.800 0.000
#&gt; GSM38758     2   0.361      1.000 0.000 0.800 0.000 0.200
#&gt; GSM38759     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38760     3   0.000      0.903 0.000 0.000 1.000 0.000
#&gt; GSM38761     2   0.361      1.000 0.000 0.800 0.000 0.200
#&gt; GSM38762     2   0.361      1.000 0.000 0.800 0.000 0.200
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.0880     0.9203 0.968 0.000 0.000 0.000 0.032
#&gt; GSM38714     1  0.4211     0.3642 0.636 0.000 0.004 0.000 0.360
#&gt; GSM38715     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     5  0.4182     0.6529 0.000 0.000 0.400 0.000 0.600
#&gt; GSM38722     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.1851     0.7748 0.000 0.000 0.912 0.000 0.088
#&gt; GSM38725     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38728     3  0.4294    -0.4308 0.000 0.000 0.532 0.000 0.468
#&gt; GSM38729     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000     0.9497 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.0000     0.8914 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38733     5  0.4182     0.6529 0.000 0.000 0.400 0.000 0.600
#&gt; GSM38734     4  0.0000     0.8470 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38735     5  0.0000     0.4817 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38736     2  0.0000     0.9984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0162     0.9953 0.000 0.996 0.000 0.004 0.000
#&gt; GSM38738     3  0.0000     0.8914 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38739     5  0.4182     0.6529 0.000 0.000 0.400 0.000 0.600
#&gt; GSM38740     5  0.4150    -0.0999 0.388 0.000 0.000 0.000 0.612
#&gt; GSM38741     3  0.0000     0.8914 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38742     2  0.0162     0.9953 0.000 0.996 0.000 0.004 0.000
#&gt; GSM38743     2  0.0000     0.9984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38744     1  0.4182     0.4812 0.600 0.000 0.000 0.000 0.400
#&gt; GSM38745     5  0.0000     0.4817 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38746     5  0.4182     0.6529 0.000 0.000 0.400 0.000 0.600
#&gt; GSM38747     5  0.5900     0.4922 0.212 0.000 0.188 0.000 0.600
#&gt; GSM38748     4  0.3895     0.5945 0.000 0.320 0.000 0.680 0.000
#&gt; GSM38749     5  0.4182     0.6529 0.000 0.000 0.400 0.000 0.600
#&gt; GSM38750     3  0.0000     0.8914 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38751     3  0.0000     0.8914 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38752     4  0.0000     0.8470 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38753     4  0.0000     0.8470 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38754     4  0.0000     0.8470 0.000 0.000 0.000 1.000 0.000
#&gt; GSM38755     3  0.0000     0.8914 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38756     4  0.4182     0.4544 0.000 0.400 0.000 0.600 0.000
#&gt; GSM38757     3  0.0000     0.8914 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38758     2  0.0000     0.9984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38759     5  0.4182     0.6529 0.000 0.000 0.400 0.000 0.600
#&gt; GSM38760     5  0.4182     0.6529 0.000 0.000 0.400 0.000 0.600
#&gt; GSM38761     2  0.0000     0.9984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000     0.9984 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.3464      0.610 0.688 0.000 0.000 0.000 0.000 0.312
#&gt; GSM38714     1  0.3634      0.536 0.644 0.000 0.000 0.000 0.000 0.356
#&gt; GSM38715     1  0.1863      0.854 0.896 0.000 0.000 0.000 0.000 0.104
#&gt; GSM38716     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.1863      0.854 0.896 0.000 0.000 0.000 0.000 0.104
#&gt; GSM38719     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38721     6  0.0000      0.620 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM38722     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.2454      0.815 0.000 0.000 0.840 0.000 0.000 0.160
#&gt; GSM38725     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38728     6  0.3998     -0.226 0.000 0.000 0.492 0.000 0.004 0.504
#&gt; GSM38729     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.936 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38733     6  0.2854      0.378 0.208 0.000 0.000 0.000 0.000 0.792
#&gt; GSM38734     4  0.0000      0.830 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38735     5  0.1327      0.682 0.000 0.000 0.000 0.000 0.936 0.064
#&gt; GSM38736     2  0.0000      0.984 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.1075      0.959 0.000 0.952 0.000 0.000 0.048 0.000
#&gt; GSM38738     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38739     6  0.3309      0.702 0.000 0.000 0.000 0.000 0.280 0.720
#&gt; GSM38740     5  0.3468      0.696 0.128 0.000 0.000 0.000 0.804 0.068
#&gt; GSM38741     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38742     2  0.1075      0.959 0.000 0.952 0.000 0.000 0.048 0.000
#&gt; GSM38743     2  0.0000      0.984 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.3592      0.512 0.344 0.000 0.000 0.000 0.656 0.000
#&gt; GSM38745     5  0.1327      0.682 0.000 0.000 0.000 0.000 0.936 0.064
#&gt; GSM38746     6  0.3309      0.702 0.000 0.000 0.000 0.000 0.280 0.720
#&gt; GSM38747     6  0.2664      0.699 0.000 0.000 0.000 0.000 0.184 0.816
#&gt; GSM38748     4  0.3888      0.610 0.000 0.312 0.000 0.672 0.016 0.000
#&gt; GSM38749     6  0.3309      0.702 0.000 0.000 0.000 0.000 0.280 0.720
#&gt; GSM38750     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38751     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38752     4  0.0000      0.830 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38753     4  0.0363      0.827 0.000 0.000 0.000 0.988 0.012 0.000
#&gt; GSM38754     4  0.0000      0.830 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM38755     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38756     4  0.4131      0.489 0.000 0.384 0.000 0.600 0.016 0.000
#&gt; GSM38757     3  0.0000      0.976 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM38758     2  0.0000      0.984 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38759     6  0.2135      0.688 0.000 0.000 0.000 0.000 0.128 0.872
#&gt; GSM38760     6  0.3309      0.702 0.000 0.000 0.000 0.000 0.280 0.720
#&gt; GSM38761     2  0.0000      0.984 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      0.984 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n individual(p) k
#> ATC:pam 51      0.048267 2
#> ATC:pam 50      0.068536 3
#> ATC:pam 50      0.005452 4
#> ATC:pam 43      0.003428 5
#> ATC:pam 48      0.000235 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.680           0.868       0.926         0.4711 0.534   0.534
#> 3 3 0.648           0.849       0.883         0.3489 0.767   0.576
#> 4 4 0.717           0.757       0.883         0.0983 0.835   0.574
#> 5 5 0.871           0.899       0.956         0.0620 0.944   0.806
#> 6 6 0.855           0.833       0.880         0.0609 0.917   0.683
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000      0.977 1.000 0.000
#&gt; GSM38713     1   0.000      0.977 1.000 0.000
#&gt; GSM38714     1   0.000      0.977 1.000 0.000
#&gt; GSM38715     1   0.000      0.977 1.000 0.000
#&gt; GSM38716     1   0.000      0.977 1.000 0.000
#&gt; GSM38717     1   0.000      0.977 1.000 0.000
#&gt; GSM38718     1   0.000      0.977 1.000 0.000
#&gt; GSM38719     1   0.000      0.977 1.000 0.000
#&gt; GSM38720     1   0.000      0.977 1.000 0.000
#&gt; GSM38721     2   0.975      0.500 0.408 0.592
#&gt; GSM38722     1   0.000      0.977 1.000 0.000
#&gt; GSM38723     1   0.000      0.977 1.000 0.000
#&gt; GSM38724     2   0.781      0.778 0.232 0.768
#&gt; GSM38725     1   0.000      0.977 1.000 0.000
#&gt; GSM38726     1   0.000      0.977 1.000 0.000
#&gt; GSM38727     1   0.000      0.977 1.000 0.000
#&gt; GSM38728     2   0.975      0.500 0.408 0.592
#&gt; GSM38729     1   0.000      0.977 1.000 0.000
#&gt; GSM38730     1   0.000      0.977 1.000 0.000
#&gt; GSM38731     1   0.000      0.977 1.000 0.000
#&gt; GSM38732     2   0.416      0.883 0.084 0.916
#&gt; GSM38733     2   0.975      0.500 0.408 0.592
#&gt; GSM38734     2   0.278      0.890 0.048 0.952
#&gt; GSM38735     2   0.000      0.887 0.000 1.000
#&gt; GSM38736     2   0.000      0.887 0.000 1.000
#&gt; GSM38737     2   0.000      0.887 0.000 1.000
#&gt; GSM38738     2   0.416      0.883 0.084 0.916
#&gt; GSM38739     2   0.781      0.778 0.232 0.768
#&gt; GSM38740     2   0.000      0.887 0.000 1.000
#&gt; GSM38741     2   0.343      0.888 0.064 0.936
#&gt; GSM38742     2   0.000      0.887 0.000 1.000
#&gt; GSM38743     2   0.000      0.887 0.000 1.000
#&gt; GSM38744     2   0.000      0.887 0.000 1.000
#&gt; GSM38745     2   0.000      0.887 0.000 1.000
#&gt; GSM38746     2   0.781      0.778 0.232 0.768
#&gt; GSM38747     1   0.909      0.372 0.676 0.324
#&gt; GSM38748     2   0.224      0.890 0.036 0.964
#&gt; GSM38749     2   0.781      0.778 0.232 0.768
#&gt; GSM38750     2   0.416      0.883 0.084 0.916
#&gt; GSM38751     2   0.402      0.885 0.080 0.920
#&gt; GSM38752     2   0.278      0.890 0.048 0.952
#&gt; GSM38753     2   0.000      0.887 0.000 1.000
#&gt; GSM38754     2   0.278      0.890 0.048 0.952
#&gt; GSM38755     2   0.416      0.883 0.084 0.916
#&gt; GSM38756     2   0.000      0.887 0.000 1.000
#&gt; GSM38757     2   0.416      0.883 0.084 0.916
#&gt; GSM38758     2   0.000      0.887 0.000 1.000
#&gt; GSM38759     2   0.921      0.624 0.336 0.664
#&gt; GSM38760     2   0.781      0.778 0.232 0.768
#&gt; GSM38761     2   0.000      0.887 0.000 1.000
#&gt; GSM38762     2   0.000      0.887 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38713     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38714     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38715     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38716     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38717     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38718     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38719     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38720     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38721     3  0.4504     0.6804 0.196 0.000 0.804
#&gt; GSM38722     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38723     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38724     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38725     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38726     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38727     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38728     3  0.4555     0.6757 0.200 0.000 0.800
#&gt; GSM38729     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38730     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38731     1  0.4291     1.0000 0.820 0.000 0.180
#&gt; GSM38732     3  0.0000     0.8147 0.000 0.000 1.000
#&gt; GSM38733     3  0.4555     0.6757 0.200 0.000 0.800
#&gt; GSM38734     3  0.7815     0.5780 0.180 0.148 0.672
#&gt; GSM38735     2  0.0747     0.9665 0.016 0.984 0.000
#&gt; GSM38736     2  0.0000     0.9695 0.000 1.000 0.000
#&gt; GSM38737     2  0.0000     0.9695 0.000 1.000 0.000
#&gt; GSM38738     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38739     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38740     2  0.0747     0.9665 0.016 0.984 0.000
#&gt; GSM38741     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38742     2  0.0000     0.9695 0.000 1.000 0.000
#&gt; GSM38743     2  0.0000     0.9695 0.000 1.000 0.000
#&gt; GSM38744     2  0.0747     0.9665 0.016 0.984 0.000
#&gt; GSM38745     2  0.0747     0.9665 0.016 0.984 0.000
#&gt; GSM38746     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38747     3  0.6308    -0.2107 0.492 0.000 0.508
#&gt; GSM38748     3  0.7829     0.5735 0.164 0.164 0.672
#&gt; GSM38749     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38750     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38751     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38752     3  0.7815     0.5780 0.180 0.148 0.672
#&gt; GSM38753     2  0.5178     0.8450 0.164 0.808 0.028
#&gt; GSM38754     3  0.7815     0.5780 0.180 0.148 0.672
#&gt; GSM38755     3  0.0000     0.8147 0.000 0.000 1.000
#&gt; GSM38756     2  0.5178     0.8450 0.164 0.808 0.028
#&gt; GSM38757     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38758     2  0.0000     0.9695 0.000 1.000 0.000
#&gt; GSM38759     3  0.7555    -0.0423 0.440 0.040 0.520
#&gt; GSM38760     3  0.1163     0.8258 0.028 0.000 0.972
#&gt; GSM38761     2  0.0000     0.9695 0.000 1.000 0.000
#&gt; GSM38762     2  0.0000     0.9695 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38714     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38715     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38716     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38720     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38721     3  0.4998    0.20426 0.488 0.000 0.512 0.000
#&gt; GSM38722     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38724     3  0.2704    0.79947 0.124 0.000 0.876 0.000
#&gt; GSM38725     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38728     1  0.4999   -0.23499 0.508 0.000 0.492 0.000
#&gt; GSM38729     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000    0.95385 1.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.2408    0.80632 0.104 0.000 0.896 0.000
#&gt; GSM38733     3  0.5000    0.16597 0.500 0.000 0.500 0.000
#&gt; GSM38734     3  0.4804   -0.00187 0.000 0.000 0.616 0.384
#&gt; GSM38735     4  0.0000    0.48979 0.000 0.000 0.000 1.000
#&gt; GSM38736     2  0.4877    1.00000 0.000 0.592 0.000 0.408
#&gt; GSM38737     2  0.4877    1.00000 0.000 0.592 0.000 0.408
#&gt; GSM38738     3  0.2408    0.80632 0.104 0.000 0.896 0.000
#&gt; GSM38739     3  0.0707    0.76700 0.020 0.000 0.980 0.000
#&gt; GSM38740     4  0.0000    0.48979 0.000 0.000 0.000 1.000
#&gt; GSM38741     3  0.0707    0.76700 0.020 0.000 0.980 0.000
#&gt; GSM38742     2  0.4877    1.00000 0.000 0.592 0.000 0.408
#&gt; GSM38743     2  0.4877    1.00000 0.000 0.592 0.000 0.408
#&gt; GSM38744     4  0.0000    0.48979 0.000 0.000 0.000 1.000
#&gt; GSM38745     4  0.0000    0.48979 0.000 0.000 0.000 1.000
#&gt; GSM38746     3  0.0707    0.76700 0.020 0.000 0.980 0.000
#&gt; GSM38747     1  0.0921    0.92396 0.972 0.000 0.028 0.000
#&gt; GSM38748     4  0.4977    0.36815 0.000 0.000 0.460 0.540
#&gt; GSM38749     3  0.0707    0.76700 0.020 0.000 0.980 0.000
#&gt; GSM38750     3  0.2408    0.80632 0.104 0.000 0.896 0.000
#&gt; GSM38751     3  0.0707    0.76700 0.020 0.000 0.980 0.000
#&gt; GSM38752     4  0.5570    0.37124 0.020 0.000 0.440 0.540
#&gt; GSM38753     4  0.4877    0.55128 0.000 0.408 0.000 0.592
#&gt; GSM38754     4  0.5570    0.37124 0.020 0.000 0.440 0.540
#&gt; GSM38755     3  0.2408    0.80632 0.104 0.000 0.896 0.000
#&gt; GSM38756     4  0.4877    0.55128 0.000 0.408 0.000 0.592
#&gt; GSM38757     3  0.2408    0.80632 0.104 0.000 0.896 0.000
#&gt; GSM38758     2  0.4877    1.00000 0.000 0.592 0.000 0.408
#&gt; GSM38759     1  0.3801    0.69909 0.780 0.000 0.000 0.220
#&gt; GSM38760     3  0.2704    0.79947 0.124 0.000 0.876 0.000
#&gt; GSM38761     2  0.4877    1.00000 0.000 0.592 0.000 0.408
#&gt; GSM38762     2  0.4877    1.00000 0.000 0.592 0.000 0.408
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4 p5
#&gt; GSM38712     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38713     1   0.029      0.963 0.992  0 0.008 0.000  0
#&gt; GSM38714     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38715     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38716     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38717     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38718     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38719     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38720     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38721     3   0.414      0.437 0.384  0 0.616 0.000  0
#&gt; GSM38722     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38723     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38724     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38725     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38726     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38727     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38728     3   0.414      0.437 0.384  0 0.616 0.000  0
#&gt; GSM38729     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38730     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38731     1   0.000      0.971 1.000  0 0.000 0.000  0
#&gt; GSM38732     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38733     3   0.414      0.437 0.384  0 0.616 0.000  0
#&gt; GSM38734     4   0.289      0.868 0.000  0 0.176 0.824  0
#&gt; GSM38735     5   0.000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38736     2   0.000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38737     2   0.000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38738     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38739     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38740     5   0.000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38741     3   0.029      0.867 0.000  0 0.992 0.008  0
#&gt; GSM38742     2   0.000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38743     2   0.000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38744     5   0.000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38745     5   0.000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM38746     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38747     1   0.179      0.880 0.916  0 0.084 0.000  0
#&gt; GSM38748     4   0.242      0.906 0.000  0 0.132 0.868  0
#&gt; GSM38749     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38750     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38751     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38752     4   0.252      0.906 0.000  0 0.140 0.860  0
#&gt; GSM38753     4   0.000      0.825 0.000  0 0.000 1.000  0
#&gt; GSM38754     4   0.252      0.906 0.000  0 0.140 0.860  0
#&gt; GSM38755     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38756     4   0.000      0.825 0.000  0 0.000 1.000  0
#&gt; GSM38757     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38758     2   0.000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38759     1   0.592      0.425 0.596  0 0.220 0.184  0
#&gt; GSM38760     3   0.000      0.875 0.000  0 1.000 0.000  0
#&gt; GSM38761     2   0.000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM38762     2   0.000      1.000 0.000  1 0.000 0.000  0
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.0000      0.928 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM38713     1  0.3592      0.400 0.656  0 0.000 0.000 0.000 0.344
#&gt; GSM38714     6  0.3672      0.528 0.368  0 0.000 0.000 0.000 0.632
#&gt; GSM38715     1  0.1267      0.917 0.940  0 0.000 0.000 0.000 0.060
#&gt; GSM38716     1  0.1267      0.917 0.940  0 0.000 0.000 0.000 0.060
#&gt; GSM38717     1  0.1267      0.917 0.940  0 0.000 0.000 0.000 0.060
#&gt; GSM38718     1  0.1267      0.917 0.940  0 0.000 0.000 0.000 0.060
#&gt; GSM38719     1  0.1267      0.917 0.940  0 0.000 0.000 0.000 0.060
#&gt; GSM38720     1  0.1267      0.917 0.940  0 0.000 0.000 0.000 0.060
#&gt; GSM38721     6  0.4482      0.594 0.360  0 0.040 0.000 0.000 0.600
#&gt; GSM38722     1  0.0000      0.928 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM38723     1  0.0146      0.926 0.996  0 0.004 0.000 0.000 0.000
#&gt; GSM38724     6  0.4602      0.518 0.044  0 0.384 0.000 0.000 0.572
#&gt; GSM38725     1  0.0000      0.928 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.928 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM38727     1  0.0000      0.928 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM38728     6  0.4504      0.583 0.368  0 0.040 0.000 0.000 0.592
#&gt; GSM38729     1  0.0000      0.928 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM38730     1  0.0000      0.928 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM38731     1  0.0000      0.928 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM38732     3  0.0000      0.956 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM38733     6  0.4482      0.594 0.360  0 0.040 0.000 0.000 0.600
#&gt; GSM38734     4  0.0547      0.870 0.000  0 0.020 0.980 0.000 0.000
#&gt; GSM38735     5  0.0146      0.994 0.000  0 0.000 0.000 0.996 0.004
#&gt; GSM38736     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM38737     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM38738     3  0.0000      0.956 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM38739     6  0.3810      0.455 0.000  0 0.428 0.000 0.000 0.572
#&gt; GSM38740     5  0.0000      0.998 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM38741     3  0.2664      0.744 0.000  0 0.816 0.184 0.000 0.000
#&gt; GSM38742     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM38743     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM38744     5  0.0000      0.998 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM38745     5  0.0000      0.998 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM38746     6  0.3810      0.455 0.000  0 0.428 0.000 0.000 0.572
#&gt; GSM38747     1  0.3782      0.543 0.740  0 0.036 0.000 0.000 0.224
#&gt; GSM38748     4  0.0363      0.869 0.000  0 0.012 0.988 0.000 0.000
#&gt; GSM38749     6  0.3810      0.455 0.000  0 0.428 0.000 0.000 0.572
#&gt; GSM38750     3  0.0260      0.955 0.000  0 0.992 0.000 0.000 0.008
#&gt; GSM38751     3  0.0000      0.956 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM38752     4  0.0547      0.870 0.000  0 0.020 0.980 0.000 0.000
#&gt; GSM38753     4  0.3647      0.725 0.000  0 0.000 0.640 0.000 0.360
#&gt; GSM38754     4  0.0547      0.870 0.000  0 0.020 0.980 0.000 0.000
#&gt; GSM38755     3  0.0260      0.955 0.000  0 0.992 0.000 0.000 0.008
#&gt; GSM38756     4  0.3647      0.725 0.000  0 0.000 0.640 0.000 0.360
#&gt; GSM38757     3  0.0260      0.955 0.000  0 0.992 0.000 0.000 0.008
#&gt; GSM38758     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM38759     6  0.4241      0.565 0.368  0 0.024 0.000 0.000 0.608
#&gt; GSM38760     6  0.4758      0.539 0.060  0 0.360 0.000 0.000 0.580
#&gt; GSM38761     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM38762     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n individual(p) k
#> ATC:mclust 47      0.004273 2
#> ATC:mclust 49      0.003068 3
#> ATC:mclust 40      0.012193 4
#> ATC:mclust 47      0.000492 5
#> ATC:mclust 47      0.002607 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 19411 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.879           0.891       0.957         0.4407 0.547   0.547
#> 3 3 0.713           0.775       0.867         0.2019 0.881   0.787
#> 4 4 0.596           0.657       0.799         0.1870 0.874   0.738
#> 5 5 0.566           0.598       0.768         0.1163 0.894   0.735
#> 6 6 0.573           0.522       0.738         0.0529 0.921   0.761
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM38712     1   0.000     0.9725 1.000 0.000
#&gt; GSM38713     1   0.000     0.9725 1.000 0.000
#&gt; GSM38714     1   0.000     0.9725 1.000 0.000
#&gt; GSM38715     1   0.000     0.9725 1.000 0.000
#&gt; GSM38716     1   0.000     0.9725 1.000 0.000
#&gt; GSM38717     1   0.000     0.9725 1.000 0.000
#&gt; GSM38718     1   0.000     0.9725 1.000 0.000
#&gt; GSM38719     1   0.000     0.9725 1.000 0.000
#&gt; GSM38720     1   0.000     0.9725 1.000 0.000
#&gt; GSM38721     1   0.000     0.9725 1.000 0.000
#&gt; GSM38722     1   0.000     0.9725 1.000 0.000
#&gt; GSM38723     1   0.000     0.9725 1.000 0.000
#&gt; GSM38724     1   0.000     0.9725 1.000 0.000
#&gt; GSM38725     1   0.000     0.9725 1.000 0.000
#&gt; GSM38726     1   0.000     0.9725 1.000 0.000
#&gt; GSM38727     1   0.000     0.9725 1.000 0.000
#&gt; GSM38728     1   0.000     0.9725 1.000 0.000
#&gt; GSM38729     1   0.000     0.9725 1.000 0.000
#&gt; GSM38730     1   0.000     0.9725 1.000 0.000
#&gt; GSM38731     1   0.000     0.9725 1.000 0.000
#&gt; GSM38732     2   0.833     0.6575 0.264 0.736
#&gt; GSM38733     1   0.000     0.9725 1.000 0.000
#&gt; GSM38734     2   0.000     0.9066 0.000 1.000
#&gt; GSM38735     1   0.000     0.9725 1.000 0.000
#&gt; GSM38736     2   0.000     0.9066 0.000 1.000
#&gt; GSM38737     2   0.000     0.9066 0.000 1.000
#&gt; GSM38738     2   0.932     0.5252 0.348 0.652
#&gt; GSM38739     1   0.000     0.9725 1.000 0.000
#&gt; GSM38740     1   0.000     0.9725 1.000 0.000
#&gt; GSM38741     2   0.973     0.4008 0.404 0.596
#&gt; GSM38742     2   0.000     0.9066 0.000 1.000
#&gt; GSM38743     2   0.000     0.9066 0.000 1.000
#&gt; GSM38744     1   0.000     0.9725 1.000 0.000
#&gt; GSM38745     1   0.000     0.9725 1.000 0.000
#&gt; GSM38746     1   0.000     0.9725 1.000 0.000
#&gt; GSM38747     1   0.000     0.9725 1.000 0.000
#&gt; GSM38748     2   0.000     0.9066 0.000 1.000
#&gt; GSM38749     1   0.000     0.9725 1.000 0.000
#&gt; GSM38750     2   0.955     0.4685 0.376 0.624
#&gt; GSM38751     1   0.925     0.4026 0.660 0.340
#&gt; GSM38752     2   0.000     0.9066 0.000 1.000
#&gt; GSM38753     2   0.000     0.9066 0.000 1.000
#&gt; GSM38754     2   0.000     0.9066 0.000 1.000
#&gt; GSM38755     1   0.141     0.9519 0.980 0.020
#&gt; GSM38756     2   0.000     0.9066 0.000 1.000
#&gt; GSM38757     1   0.988     0.0949 0.564 0.436
#&gt; GSM38758     2   0.000     0.9066 0.000 1.000
#&gt; GSM38759     1   0.000     0.9725 1.000 0.000
#&gt; GSM38760     1   0.000     0.9725 1.000 0.000
#&gt; GSM38761     2   0.000     0.9066 0.000 1.000
#&gt; GSM38762     2   0.000     0.9066 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM38712     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38713     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38714     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38715     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38716     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38717     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38718     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38719     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38720     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38721     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38722     1  0.0592      0.929 0.988 0.012 0.000
#&gt; GSM38723     1  0.0592      0.929 0.988 0.012 0.000
#&gt; GSM38724     1  0.1031      0.913 0.976 0.000 0.024
#&gt; GSM38725     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38726     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38727     1  0.0592      0.929 0.988 0.012 0.000
#&gt; GSM38728     1  0.0237      0.932 0.996 0.000 0.004
#&gt; GSM38729     1  0.0237      0.934 0.996 0.004 0.000
#&gt; GSM38730     1  0.0237      0.934 0.996 0.004 0.000
#&gt; GSM38731     1  0.0424      0.932 0.992 0.008 0.000
#&gt; GSM38732     3  0.5363      0.630 0.276 0.000 0.724
#&gt; GSM38733     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38734     3  0.0000      0.557 0.000 0.000 1.000
#&gt; GSM38735     2  0.4291      0.362 0.180 0.820 0.000
#&gt; GSM38736     2  0.5835      0.875 0.000 0.660 0.340
#&gt; GSM38737     2  0.5760      0.871 0.000 0.672 0.328
#&gt; GSM38738     3  0.5785      0.622 0.332 0.000 0.668
#&gt; GSM38739     1  0.0424      0.932 0.992 0.008 0.000
#&gt; GSM38740     1  0.4235      0.761 0.824 0.176 0.000
#&gt; GSM38741     3  0.5968      0.589 0.364 0.000 0.636
#&gt; GSM38742     2  0.5810      0.876 0.000 0.664 0.336
#&gt; GSM38743     2  0.5810      0.876 0.000 0.664 0.336
#&gt; GSM38744     1  0.3482      0.818 0.872 0.128 0.000
#&gt; GSM38745     1  0.6308      0.209 0.508 0.492 0.000
#&gt; GSM38746     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38747     1  0.0237      0.934 0.996 0.004 0.000
#&gt; GSM38748     3  0.0424      0.551 0.000 0.008 0.992
#&gt; GSM38749     1  0.0424      0.932 0.992 0.008 0.000
#&gt; GSM38750     3  0.5760      0.623 0.328 0.000 0.672
#&gt; GSM38751     1  0.5785      0.343 0.668 0.000 0.332
#&gt; GSM38752     3  0.0000      0.557 0.000 0.000 1.000
#&gt; GSM38753     3  0.4087      0.529 0.052 0.068 0.880
#&gt; GSM38754     3  0.0237      0.554 0.000 0.004 0.996
#&gt; GSM38755     1  0.6307     -0.250 0.512 0.000 0.488
#&gt; GSM38756     3  0.2878      0.414 0.000 0.096 0.904
#&gt; GSM38757     3  0.6045      0.555 0.380 0.000 0.620
#&gt; GSM38758     2  0.6309      0.654 0.000 0.500 0.500
#&gt; GSM38759     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38760     1  0.0000      0.935 1.000 0.000 0.000
#&gt; GSM38761     2  0.5835      0.875 0.000 0.660 0.340
#&gt; GSM38762     2  0.5810      0.876 0.000 0.664 0.336
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM38712     1  0.0376     0.9027 0.992 0.004 0.004 0.000
#&gt; GSM38713     1  0.0376     0.9024 0.992 0.004 0.004 0.000
#&gt; GSM38714     1  0.0469     0.9035 0.988 0.012 0.000 0.000
#&gt; GSM38715     1  0.0376     0.9024 0.992 0.004 0.004 0.000
#&gt; GSM38716     1  0.0000     0.9032 1.000 0.000 0.000 0.000
#&gt; GSM38717     1  0.0000     0.9032 1.000 0.000 0.000 0.000
#&gt; GSM38718     1  0.0000     0.9032 1.000 0.000 0.000 0.000
#&gt; GSM38719     1  0.0188     0.9035 0.996 0.004 0.000 0.000
#&gt; GSM38720     1  0.0188     0.9029 0.996 0.000 0.004 0.000
#&gt; GSM38721     1  0.1743     0.8792 0.940 0.000 0.056 0.004
#&gt; GSM38722     1  0.0779     0.9021 0.980 0.016 0.004 0.000
#&gt; GSM38723     1  0.1109     0.8989 0.968 0.028 0.004 0.000
#&gt; GSM38724     1  0.2958     0.8287 0.876 0.004 0.116 0.004
#&gt; GSM38725     1  0.0657     0.9028 0.984 0.012 0.004 0.000
#&gt; GSM38726     1  0.0188     0.9032 0.996 0.000 0.004 0.000
#&gt; GSM38727     1  0.1489     0.8931 0.952 0.044 0.004 0.000
#&gt; GSM38728     1  0.6179     0.4821 0.664 0.012 0.256 0.068
#&gt; GSM38729     1  0.0188     0.9039 0.996 0.004 0.000 0.000
#&gt; GSM38730     1  0.0376     0.9036 0.992 0.004 0.004 0.000
#&gt; GSM38731     1  0.0524     0.9034 0.988 0.008 0.004 0.000
#&gt; GSM38732     3  0.2111     0.7297 0.024 0.000 0.932 0.044
#&gt; GSM38733     1  0.4262     0.6447 0.756 0.008 0.236 0.000
#&gt; GSM38734     3  0.3486     0.6671 0.000 0.000 0.812 0.188
#&gt; GSM38735     2  0.4485     0.2830 0.200 0.772 0.000 0.028
#&gt; GSM38736     4  0.5404     0.4846 0.000 0.476 0.012 0.512
#&gt; GSM38737     2  0.5155    -0.6462 0.000 0.528 0.004 0.468
#&gt; GSM38738     3  0.2089     0.7316 0.020 0.012 0.940 0.028
#&gt; GSM38739     1  0.4595     0.7405 0.776 0.184 0.040 0.000
#&gt; GSM38740     1  0.4761     0.5043 0.664 0.332 0.004 0.000
#&gt; GSM38741     3  0.5984     0.5826 0.048 0.000 0.580 0.372
#&gt; GSM38742     4  0.4996     0.4823 0.000 0.484 0.000 0.516
#&gt; GSM38743     4  0.5165     0.4792 0.000 0.484 0.004 0.512
#&gt; GSM38744     1  0.3726     0.7312 0.788 0.212 0.000 0.000
#&gt; GSM38745     2  0.5060     0.1668 0.412 0.584 0.004 0.000
#&gt; GSM38746     1  0.3215     0.8550 0.888 0.076 0.016 0.020
#&gt; GSM38747     1  0.1833     0.8904 0.944 0.024 0.000 0.032
#&gt; GSM38748     4  0.4356     0.1263 0.000 0.000 0.292 0.708
#&gt; GSM38749     1  0.3812     0.8053 0.832 0.140 0.028 0.000
#&gt; GSM38750     3  0.2329     0.7306 0.024 0.020 0.932 0.024
#&gt; GSM38751     3  0.8048     0.1993 0.360 0.088 0.484 0.068
#&gt; GSM38752     3  0.4972     0.5074 0.000 0.000 0.544 0.456
#&gt; GSM38753     4  0.6181     0.0577 0.128 0.000 0.204 0.668
#&gt; GSM38754     3  0.5132     0.4796 0.000 0.004 0.548 0.448
#&gt; GSM38755     3  0.2839     0.6842 0.108 0.004 0.884 0.004
#&gt; GSM38756     4  0.3873     0.2336 0.000 0.000 0.228 0.772
#&gt; GSM38757     3  0.1510     0.7330 0.028 0.000 0.956 0.016
#&gt; GSM38758     4  0.4934     0.4863 0.000 0.252 0.028 0.720
#&gt; GSM38759     1  0.3027     0.8444 0.888 0.088 0.004 0.020
#&gt; GSM38760     1  0.3959     0.8138 0.840 0.068 0.092 0.000
#&gt; GSM38761     4  0.4972     0.4917 0.000 0.456 0.000 0.544
#&gt; GSM38762     4  0.4998     0.4777 0.000 0.488 0.000 0.512
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM38712     1  0.0693     0.7997 0.980 0.000 0.000 0.008 0.012
#&gt; GSM38713     1  0.1469     0.7877 0.948 0.000 0.000 0.016 0.036
#&gt; GSM38714     1  0.1443     0.7985 0.948 0.000 0.004 0.004 0.044
#&gt; GSM38715     1  0.2074     0.7709 0.920 0.000 0.004 0.016 0.060
#&gt; GSM38716     1  0.0609     0.7999 0.980 0.000 0.000 0.000 0.020
#&gt; GSM38717     1  0.0898     0.7987 0.972 0.000 0.000 0.008 0.020
#&gt; GSM38718     1  0.0992     0.7955 0.968 0.000 0.000 0.008 0.024
#&gt; GSM38719     1  0.1251     0.7895 0.956 0.000 0.000 0.008 0.036
#&gt; GSM38720     1  0.1082     0.7923 0.964 0.000 0.000 0.008 0.028
#&gt; GSM38721     1  0.2968     0.7437 0.872 0.000 0.092 0.008 0.028
#&gt; GSM38722     1  0.1478     0.7888 0.936 0.000 0.000 0.000 0.064
#&gt; GSM38723     1  0.3462     0.6667 0.792 0.000 0.000 0.012 0.196
#&gt; GSM38724     1  0.4578     0.6759 0.784 0.000 0.048 0.048 0.120
#&gt; GSM38725     1  0.1965     0.7711 0.904 0.000 0.000 0.000 0.096
#&gt; GSM38726     1  0.2011     0.7756 0.908 0.000 0.000 0.004 0.088
#&gt; GSM38727     1  0.3171     0.7007 0.816 0.000 0.000 0.008 0.176
#&gt; GSM38728     1  0.5411     0.2125 0.560 0.000 0.392 0.024 0.024
#&gt; GSM38729     1  0.0404     0.7996 0.988 0.000 0.000 0.000 0.012
#&gt; GSM38730     1  0.1041     0.7962 0.964 0.000 0.000 0.004 0.032
#&gt; GSM38731     1  0.1043     0.7954 0.960 0.000 0.000 0.000 0.040
#&gt; GSM38732     3  0.1356     0.5383 0.004 0.000 0.956 0.028 0.012
#&gt; GSM38733     1  0.3992     0.5153 0.712 0.000 0.280 0.004 0.004
#&gt; GSM38734     3  0.4387     0.2330 0.000 0.012 0.640 0.348 0.000
#&gt; GSM38735     5  0.7802     0.3488 0.164 0.300 0.000 0.104 0.432
#&gt; GSM38736     2  0.0324     0.9072 0.000 0.992 0.000 0.004 0.004
#&gt; GSM38737     2  0.2574     0.8314 0.000 0.876 0.000 0.012 0.112
#&gt; GSM38738     3  0.5001     0.5693 0.004 0.000 0.700 0.080 0.216
#&gt; GSM38739     5  0.4996     0.5983 0.228 0.000 0.052 0.016 0.704
#&gt; GSM38740     1  0.5347    -0.1186 0.528 0.004 0.000 0.044 0.424
#&gt; GSM38741     4  0.7388    -0.1081 0.048 0.000 0.312 0.444 0.196
#&gt; GSM38742     2  0.1082     0.9022 0.000 0.964 0.000 0.028 0.008
#&gt; GSM38743     2  0.0404     0.9076 0.000 0.988 0.000 0.000 0.012
#&gt; GSM38744     1  0.4836     0.2099 0.628 0.000 0.000 0.036 0.336
#&gt; GSM38745     5  0.7418     0.5331 0.280 0.188 0.000 0.060 0.472
#&gt; GSM38746     5  0.5901     0.3725 0.436 0.000 0.032 0.040 0.492
#&gt; GSM38747     1  0.3723     0.7391 0.840 0.000 0.032 0.040 0.088
#&gt; GSM38748     4  0.5805     0.5641 0.000 0.200 0.132 0.652 0.016
#&gt; GSM38749     5  0.5211     0.5865 0.320 0.000 0.040 0.012 0.628
#&gt; GSM38750     3  0.6621     0.4246 0.044 0.000 0.516 0.092 0.348
#&gt; GSM38751     5  0.6162     0.3299 0.140 0.000 0.204 0.028 0.628
#&gt; GSM38752     3  0.5552     0.1615 0.000 0.024 0.596 0.340 0.040
#&gt; GSM38753     4  0.5817     0.5207 0.088 0.092 0.040 0.732 0.048
#&gt; GSM38754     3  0.6341     0.1797 0.000 0.116 0.592 0.260 0.032
#&gt; GSM38755     3  0.5212     0.5538 0.036 0.000 0.732 0.084 0.148
#&gt; GSM38756     4  0.4788     0.5845 0.000 0.240 0.064 0.696 0.000
#&gt; GSM38757     3  0.5153     0.5697 0.024 0.000 0.708 0.060 0.208
#&gt; GSM38758     2  0.3684     0.6877 0.000 0.788 0.004 0.192 0.016
#&gt; GSM38759     1  0.4025     0.6869 0.796 0.000 0.004 0.060 0.140
#&gt; GSM38760     1  0.5951     0.0269 0.552 0.000 0.076 0.016 0.356
#&gt; GSM38761     2  0.1717     0.8763 0.000 0.936 0.004 0.052 0.008
#&gt; GSM38762     2  0.1059     0.9019 0.000 0.968 0.004 0.008 0.020
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM38712     1  0.1074    0.83319 0.960 0.000 0.012 0.000 0.028 0.000
#&gt; GSM38713     1  0.1340    0.82882 0.948 0.000 0.008 0.004 0.040 0.000
#&gt; GSM38714     1  0.1644    0.82139 0.920 0.000 0.004 0.000 0.076 0.000
#&gt; GSM38715     1  0.1801    0.82017 0.924 0.000 0.016 0.004 0.056 0.000
#&gt; GSM38716     1  0.1088    0.83436 0.960 0.000 0.016 0.000 0.024 0.000
#&gt; GSM38717     1  0.0935    0.83495 0.964 0.000 0.004 0.000 0.032 0.000
#&gt; GSM38718     1  0.1320    0.82859 0.948 0.000 0.016 0.000 0.036 0.000
#&gt; GSM38719     1  0.0964    0.83384 0.968 0.000 0.012 0.004 0.016 0.000
#&gt; GSM38720     1  0.1320    0.82882 0.948 0.000 0.016 0.000 0.036 0.000
#&gt; GSM38721     1  0.2495    0.81491 0.896 0.000 0.012 0.004 0.052 0.036
#&gt; GSM38722     1  0.1625    0.82350 0.928 0.000 0.060 0.000 0.012 0.000
#&gt; GSM38723     1  0.3796    0.69566 0.764 0.000 0.176 0.000 0.060 0.000
#&gt; GSM38724     1  0.4118    0.75221 0.804 0.000 0.088 0.052 0.040 0.016
#&gt; GSM38725     1  0.1858    0.81450 0.912 0.000 0.076 0.000 0.012 0.000
#&gt; GSM38726     1  0.1789    0.82554 0.924 0.000 0.044 0.000 0.032 0.000
#&gt; GSM38727     1  0.3236    0.75364 0.820 0.000 0.140 0.004 0.036 0.000
#&gt; GSM38728     6  0.5360    0.02568 0.420 0.000 0.004 0.032 0.036 0.508
#&gt; GSM38729     1  0.1196    0.83568 0.952 0.000 0.008 0.000 0.040 0.000
#&gt; GSM38730     1  0.1168    0.83147 0.956 0.000 0.028 0.000 0.016 0.000
#&gt; GSM38731     1  0.0909    0.83423 0.968 0.000 0.020 0.000 0.012 0.000
#&gt; GSM38732     6  0.4701   -0.27992 0.000 0.000 0.028 0.480 0.008 0.484
#&gt; GSM38733     1  0.4763    0.67806 0.756 0.000 0.016 0.100 0.044 0.084
#&gt; GSM38734     4  0.3328    0.29388 0.000 0.008 0.000 0.788 0.012 0.192
#&gt; GSM38735     3  0.7723    0.19102 0.124 0.296 0.340 0.004 0.228 0.008
#&gt; GSM38736     2  0.2130    0.89239 0.000 0.920 0.008 0.028 0.016 0.028
#&gt; GSM38737     2  0.1755    0.86459 0.000 0.932 0.028 0.008 0.032 0.000
#&gt; GSM38738     4  0.6452    0.24069 0.000 0.000 0.300 0.384 0.016 0.300
#&gt; GSM38739     3  0.2915    0.51319 0.120 0.000 0.848 0.000 0.008 0.024
#&gt; GSM38740     3  0.6435    0.29221 0.368 0.020 0.412 0.000 0.196 0.004
#&gt; GSM38741     6  0.6310    0.09403 0.036 0.000 0.344 0.036 0.068 0.516
#&gt; GSM38742     2  0.0696    0.89942 0.000 0.980 0.004 0.004 0.004 0.008
#&gt; GSM38743     2  0.0405    0.90198 0.000 0.988 0.000 0.008 0.004 0.000
#&gt; GSM38744     1  0.6340   -0.22809 0.428 0.016 0.316 0.000 0.240 0.000
#&gt; GSM38745     3  0.7193    0.39734 0.188 0.140 0.468 0.000 0.200 0.004
#&gt; GSM38746     3  0.6020    0.47025 0.252 0.000 0.580 0.000 0.076 0.092
#&gt; GSM38747     1  0.5192    0.50069 0.644 0.000 0.072 0.000 0.032 0.252
#&gt; GSM38748     4  0.5133   -0.21798 0.000 0.052 0.008 0.716 0.128 0.096
#&gt; GSM38749     3  0.3469    0.51115 0.168 0.000 0.800 0.008 0.016 0.008
#&gt; GSM38750     3  0.6416   -0.00903 0.048 0.000 0.544 0.156 0.008 0.244
#&gt; GSM38751     3  0.4251    0.41257 0.076 0.000 0.768 0.008 0.012 0.136
#&gt; GSM38752     6  0.2372    0.29920 0.004 0.016 0.012 0.024 0.032 0.912
#&gt; GSM38753     5  0.6783    0.00000 0.044 0.012 0.012 0.304 0.500 0.128
#&gt; GSM38754     6  0.3174    0.29434 0.004 0.124 0.012 0.016 0.004 0.840
#&gt; GSM38755     4  0.6570    0.33333 0.072 0.000 0.152 0.596 0.032 0.148
#&gt; GSM38756     4  0.6701   -0.47218 0.000 0.080 0.020 0.540 0.252 0.108
#&gt; GSM38757     4  0.6465    0.31156 0.032 0.000 0.268 0.492 0.004 0.204
#&gt; GSM38758     2  0.4802    0.77082 0.000 0.760 0.020 0.084 0.068 0.068
#&gt; GSM38759     1  0.5655    0.39339 0.588 0.000 0.096 0.000 0.280 0.036
#&gt; GSM38760     1  0.5776    0.06103 0.500 0.000 0.396 0.068 0.028 0.008
#&gt; GSM38761     2  0.3417    0.85098 0.000 0.852 0.016 0.048 0.052 0.032
#&gt; GSM38762     2  0.1520    0.89914 0.000 0.948 0.008 0.020 0.016 0.008
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n individual(p) k
#> ATC:NMF 47      0.094790 2
#> ATC:NMF 46      0.002682 3
#> ATC:NMF 36      0.068195 4
#> ATC:NMF 39      0.014891 5
#> ATC:NMF 30      0.000921 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] genefilter_1.66.0    ComplexHeatmap_2.3.1 markdown_1.1         knitr_1.26          
#> [5] GetoptLong_0.1.7     cola_1.3.2          
#> 
#> loaded via a namespace (and not attached):
#>  [1] circlize_0.4.8       shape_1.4.4          xfun_0.11            slam_0.1-46         
#>  [5] lattice_0.20-38      splines_3.6.0        colorspace_1.4-1     vctrs_0.2.0         
#>  [9] stats4_3.6.0         blob_1.2.0           XML_3.98-1.20        survival_2.44-1.1   
#> [13] rlang_0.4.2          pillar_1.4.2         DBI_1.0.0            BiocGenerics_0.30.0 
#> [17] bit64_0.9-7          RColorBrewer_1.1-2   matrixStats_0.55.0   stringr_1.4.0       
#> [21] GlobalOptions_0.1.1  evaluate_0.14        memoise_1.1.0        Biobase_2.44.0      
#> [25] IRanges_2.18.3       parallel_3.6.0       AnnotationDbi_1.46.1 highr_0.8           
#> [29] Rcpp_1.0.3           xtable_1.8-4         backports_1.1.5      S4Vectors_0.22.1    
#> [33] annotate_1.62.0      skmeans_0.2-11       bit_1.1-14           microbenchmark_1.4-7
#> [37] brew_1.0-6           impute_1.58.0        rjson_0.2.20         png_0.1-7           
#> [41] digest_0.6.23        stringi_1.4.3        polyclip_1.10-0      clue_0.3-57         
#> [45] tools_3.6.0          bitops_1.0-6         magrittr_1.5         eulerr_6.0.0        
#> [49] RCurl_1.95-4.12      RSQLite_2.1.4        tibble_2.1.3         cluster_2.1.0       
#> [53] crayon_1.3.4         pkgconfig_2.0.3      zeallot_0.1.0        Matrix_1.2-17       
#> [57] xml2_1.2.2           httr_1.4.1           R6_2.4.1             mclust_5.4.5        
#> [61] compiler_3.6.0
```


