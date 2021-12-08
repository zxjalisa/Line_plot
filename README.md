# lineplot
####Line plot for each gene
gg_list=c("") ##gene name  eg, gg_list=resFilter$symbol %>% as.character() %>% .[1:100]
gg_list=gg_list[!is.na(gg_list)]
pp=list()
for (gene in gg_list) {
  gg=filter(res %>% as.data.frame(),symbol==gene) %>% rownames()
  pp[[gene]] <- plotCounts(ddsTC, gene=gg, intgroup="Group", returnData=TRUE) %>% mutate(id=gsub("_.*","",rownames(.))) %>% 
    ggplot(aes(x=fct_relevel(Group,levels=c("treatment1", "treatment2" , "control")),y=count,group=id,colour=id))+geom_line(aes(group=id))+ scale_y_log10() + ggtitle(gene)+theme_bw()+geom_jitter(aes(color=id),width = 0.2,size=2)+scale_color_jama()+xlab("Group")
}
library(cowplot)
library(gridExtra)
glist <- lapply(pp, ggplotGrob)
ggsave("Line_plots.pdf", width = 12,height = 12,marrangeGrob(glist, nrow = 3, ncol = 3))
