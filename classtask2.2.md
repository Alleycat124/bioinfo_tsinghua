```R
library(tidyverse)

head(iris)
iris$Species <- as.factor(iris$Species)

ggplot(iris, aes(x=Species, y=Sepal.Length, fill = Species))+
  geom_violin(trim = FALSE)+
  scale_fill_manual(values = c('#c44e52', '#55A868', '#4C72B0'))+
  stat_summary(fun.data="mean_sdl", fun.args = list(mult = 1), geom="crossbar", width=0.1 )+
  labs(title = "Sepal Length Distribution")+
  theme(legend.position="right",
        panel.grid=element_blank(),
        plot.title = element_text(face = "bold", hjust = 0.5))+
  scale_y_continuous(limits = c(3,9))
```


![image](https://github.com/user-attachments/assets/12eca677-81c3-4ddc-9ff0-2ed173aacf47)
