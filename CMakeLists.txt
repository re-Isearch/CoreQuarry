cmake_minimum_required(VERSION 3.20)
project(CoreQuarryWorkspace)

# Add Schmate first so its targets (sbert_extended, bert_model) are defined
add_subdirectory(Schmate)
add_subdirectory(ib)
