# Leafi_extended
An extended work of Leafi (index: dstree, ML models: CNN, MLP)

# Compile
mkdir build  

rm -rf *

cmake ..

make


# Run
#  Build index，train filter, using dump to store results
./dstree \
  --db_filepath /data/qiyanlin/qitong/dataset/deep1b-96-1m.bin \
  --query_filepath /data/qiyanlin/qitong/dataset/deep1b-96-10m-test-0.4-10k.bin \
  --log_filepath /data/qiyanlin/qitong/dstree/log/big_MLP_LeafSize10000.log \
  --series_length 96 \
  --db_size 1000000 \
  --query_size 10000 \
  --leaf_size 10000 \
  --exact_search \
  --n_nearest_neighbor 10 \
  --require_neurofilter \
  --filter_train_is_gpu \
  --filter_infer_is_gpu \
  --filter_query_filepath /data/qiyanlin/qitong/dataset/deep1b-96-1m_filter_10k.bin \
  --filter_train_nexample 100 \
  --learning_rate 0.01 \
  --filter_train_min_lr 0.000001 \
  --filter_train_clip_grad \
  --filter_train_nepoch 100 \
  --filter_train_mthread \
  --filter_collect_nthread 16 \
  --filter_train_nthread 16 \
  --filter_remove_square \
  --filter_is_conformal \
  --filter_train_val_split 0.6 \
  --filter_model_setting mlp \
  --filter_conformal_recall 0.7 \
  --filter_allocate_is_gain \
  --device_id 0 \
  --dump_index \
  --index_dump_folderpath /data/qiyanlin/qitong/dstree/index_dump_1M


# only loading results that firstly built，not rebuild index，not retrain filter
--load_index --load_filters --index_load_folderpath  
--filter_train_val_split 0.6   
 The number of Calibration set for Conformal Prediction = filter_train_nexample*(1-filter_train_val_split)
 filter_conformal_is_smoothened: spline regression, countinuous method
  
./dstree \
  --db_filepath /data/qiyanlin/qitong/dataset/deep1b-96-1m.bin \
  --query_filepath /data/qiyanlin/qitong/dataset/deep1b-96-10m-test-0.4-2.bin \
  --log_filepath /data/qiyanlin/qitong/dstree/log/load_query10_db1m.log \
  --series_length 96 \
  --db_size 1000000 \
  --query_size 2 \
  --leaf_size 10000 \
  --exact_search \
  --n_nearest_neighbor 10 \
  --require_neurofilter \
  --filter_train_is_gpu \
  --filter_infer_is_gpu \
  --filter_query_filepath /data/qiyanlin/qitong/dataset/deep1b-96-1m_filter_10k.bin \
  --filter_train_nexample 100 \
  --learning_rate 0.01 \
  --filter_train_min_lr 0.000001 \
  --filter_train_clip_grad \
  --filter_train_nepoch 100 \
  --filter_train_mthread \
  --filter_collect_nthread 16 \
  --filter_train_nthread 16 \
  --filter_remove_square \
  --filter_is_conformal \
  --filter_train_val_split 0.9 \
  --filter_model_setting mlp \
  --filter_conformal_recall 0.999 \
  --filter_allocate_is_gain \
  --device_id 0 \
  --load_index \
  --load_filters \
  --index_load_folderpath /data/qiyanlin/qitong/dstree/index_dump_1M \
  --filter_conformal_is_smoothened
