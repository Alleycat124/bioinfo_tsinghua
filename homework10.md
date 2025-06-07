# 1

```R
library(glmnet)
library(caret)
library(pROC)
#BiocManager::install("mlbench")
library(mlbench)
#BiocManager::install("randomForest")
library(randomForest)
#BiocManager::install("ggfortify")

#读取数据
filepath <- file.choose()
#filepath <- "E:\\LiuXing\\bioinfo_tsinghua\\DATA_FOR_R\\qPCR_data.csv"
PCR_data <- read.csv(filepath)

#数据预处理
label <- PCR_data$label
x <- PCR_data[,2:12]
x <- apply(x, 2, as.numeric)
feature.mean <- colMeans(x, na.rm = T)
x[is.na(x)] <- matrix(rep(feature.mean, each=length(label)), 
                      nrow = length(label))[is.na(x)]
x <- scale(x,
           center = T, 
           scale = T)


#数据可视化
PCR_data_forPCA <- PCR_data
PCR_data_forPCA[,2:12] <- x
as.data.frame(PCR_data_forPCA)
row.names(PCR_data_forPCA) <- PCR_data_forPCA$sample_id
#PCR_data_forPCA <- PCR_data_forPCA[,-1]

pca <- prcomp(PCR_data_forPCA[2:12], scale=T)
percentVar <- pca$sdev^2/sum(pca$sdev^2)
print(str(pca))

library(ggfortify)
autoplot(pca, data=PCR_data_forPCA, colour="label") + 
  xlab(paste0("PC1 (", round(percentVar[1]*100), "% variance)")) + 
  ylab(paste0("PC2 (", round(percentVar[2]*100), "% variance)")) + theme_bw() + 
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank()) + 
  theme(legend.position="right")


#数据集划分
set.seed(124)
#80%用于训练，20%用于确保泛化能力
train.indices <- createDataPartition(label, 
                                     p=0.8,
                                     times = 1,
                                     list = T)$Resample1
x.train <- x[train.indices,]
x.test <- x[-train.indices,]
label.train <- label[train.indices]
label.test <- label[-train.indices]

#特征选择
rfFuncs$summary <- twoClassSummary
rfectrl <- rfeControl(functions = rfFuncs,
                      verbose = T,
                      method = "boot",
                      number = 10)
rfe.results <- rfe(x.train, factor(label.train),
                   sizes = 2:11,
                   rfeControl = rfectrl,
                   metric = "ROC")
selected.features <- predictors(rfe.results)
selected.features
# "LINC01226"        "miR.122"          "SNORD3B"          "hsa_circ_0073052"

#调参和模型拟合
# #
# for (alpha in seq(0.01,1,0.01)) {
#   cv_fit <- cv.glmnet(x = exp_mat, y = sample_df$age, alpha = alpha, nfolds = nrow(exp_mat))
#   if (min(cv_fit$cvm) < best_cv_error) {
#     best_cv_error <- min(cv_fit$cvm)
#     best_alpha <- alpha
#     best_lambda <- cv_fit$lambda.min
#   }
# }
# print(paste("Best alpha:", best_alpha))
# print(paste("Best lambda:", best_lambda))

params.grid <- expand.grid(alpha=seq(0.01,1,0.01),
                           lambda=seq(0.01,1,0.01))
tr.ctrl <- trainControl(method = "cv",
                        number = 5,
                        summaryFunction = twoClassSummary,
                        classProbs = T)
cv.fitted <- train(x.train[,predictors(rfe.results)], label.train,
                   method="glmnet",
                   family="binomial",
                   metric = "ROC",
                   tuneGrid = params.grid,
                   preProcess = NULL,
                   trControl = tr.ctrl )

cv.fitted$bestTune
# alpha lambda
# 4705  0.48   0.05

#模型性能评估
label.test.pred.prob <- predict(cv.fitted,newdata=x.test,type="prob")
roc.curve <- roc(label.test,label.test.pred.prob[,2])
plot(roc.curve,print.auc=T)

```

![homework_10_PCA](https://github.com/user-attachments/assets/5a349332-c867-4f93-ba76-a42028db99a2)

![homework_10_ROC](https://github.com/user-attachments/assets/1e9ac2f2-0a03-4144-88a9-d367551ad51d)



# 2
1. 树的数量需要通过交叉验证调整
原因：

模型性能变化：增加树的数量通常会提升模型的稳定性和准确性，但当达到一定数量后，性能提升会变得逐渐平缓，甚至不明显。  
计算成本：树越多，训练和预测的时间越长，因此需要权衡效果和资源。  
优化超参数：通过交叉验证可以找到一个折中的树的数量，使模型既保证了良好的性能，又不至于不必要地增加计算负担。  

2. OOB误差（Out-of-Bag Error）：是在训练随机森林过程中，不同决策树在训练时未用到的样本（即“袋外样本”）上的预测错误率。

和Bootstrapping的关系：

Bootstrapping（自助采样）：在训练随机森林时，构建每棵树的样本集都是通过有放回的随机采样（即bootstrapping）得到。
关系：每棵树用的样本是通过bootstrapping获得的，而未被采样到的样本（在每次采样过程中没有被包括的样本）即构成了袋外样本。
因此，bootstrapping生成训练样本集的同时，也定义了“袋外样本”的集合。
