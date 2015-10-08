# Copyright (C) 2014 Ion Torrent Systems, Inc. All Rights Reserved
cmake_minimum_required (VERSION 2.6)
project (ion-tvc)


set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-msse3")
add_definitions(-DALIGNSTATS_IGNORE)

configure_file (
        "${PROJECT_SOURCE_DIR}/IonVersion.cpp.in"
        "${PROJECT_BINARY_DIR}/IonVersion.cpp" @ONLY
)


set(ION_VCFLIB_DIR    external/vcflib)
set(ION_FREEBAYES_DIR external/freebayes)
set(ION_JSONCPP_DIR   external/jsoncpp-src-amalgated0.6.0-rc1)
set(ION_BAMTOOLS_DIR  ${PROJECT_BINARY_DIR}/../bamtools-2.4.0.20150702+git15eadb925f)
set(ION_BAMTOOLS_LIBS ${PROJECT_BINARY_DIR}/../bamtools-2.4.0.20150702+git15eadb925f-build/lib/libbamtools.a)
set(ION_ARMADILLO_DIR ${PROJECT_BINARY_DIR}/../armadillo-4.600.1)

include_directories(${ION_VCFLIB_DIR})
include_directories(${ION_FREEBAYES_DIR}/src)
include_directories(${ION_JSONCPP_DIR})
include_directories("${ION_ARMADILLO_DIR}/include")
include_directories("${ION_BAMTOOLS_DIR}/src")

include_directories("${PROJECT_SOURCE_DIR}/VariantCaller")
include_directories("${PROJECT_SOURCE_DIR}/VariantCaller/FlowDistEval")
include_directories("${PROJECT_SOURCE_DIR}/VariantCaller/EnsembleEval")
include_directories("${PROJECT_SOURCE_DIR}/VariantCaller/Bookkeeping")
include_directories("${PROJECT_SOURCE_DIR}/VariantCaller/Reads")
include_directories("${PROJECT_SOURCE_DIR}/VariantCaller/Splice")
include_directories("${PROJECT_SOURCE_DIR}/VariantCaller/Filter")
include_directories("${PROJECT_SOURCE_DIR}/realignment")
include_directories("${PROJECT_SOURCE_DIR}/file-io")
include_directories("${PROJECT_SOURCE_DIR}/BaseCaller")
include_directories("${PROJECT_SOURCE_DIR}/Calibration")
include_directories("${PROJECT_SOURCE_DIR}/Util")
include_directories("${PROJECT_SOURCE_DIR}")

add_executable(tvc
  VariantCaller/VariantCaller.cpp
  VariantCaller/BAMWalkerEngine.cpp
  VariantCaller/OrderedBAMWriter.cpp
  VariantCaller/SampleManager.cpp
  VariantCaller/TargetsManager.cpp
  VariantCaller/HandleVariant.cpp
  VariantCaller/HotspotReader.cpp
  VariantCaller/MetricsManager.cpp

  VariantCaller/Bookkeeping/MiscUtil.cpp
  VariantCaller/Bookkeeping/ExtendParameters.cpp 
  VariantCaller/Bookkeeping/InputStructures.cpp
  VariantCaller/Bookkeeping/VcfFormat.cpp

  VariantCaller/Reads/ExtendedReadInfo.cpp

  VariantCaller/Splice/ErrorMotifs.cpp
  VariantCaller/Splice/LocalContext.cpp
  VariantCaller/Splice/ClassifyVariant.cpp
  VariantCaller/Splice/ErrorMotifs.cpp
  VariantCaller/Splice/SpliceVariantHypotheses.cpp

  VariantCaller/Filter/DecisionTreeData.cpp
  VariantCaller/Filter/VariantAssist.cpp

  VariantCaller/HypothesisEvaluator.cpp

  VariantCaller/EnsembleEval/DiagnosticJSON.cpp
  VariantCaller/EnsembleEval/BiasGenerator.cpp
  VariantCaller/EnsembleEval/SigmaGenerator.cpp
  VariantCaller/EnsembleEval/SkewGenerator.cpp
  VariantCaller/EnsembleEval/PosteriorInference.cpp
  VariantCaller/EnsembleEval/ShortStack.cpp
  VariantCaller/EnsembleEval/StackEngine.cpp
  VariantCaller/EnsembleEval/CrossHypotheses.cpp

  # TODO: Actually build vcflib as a static library and link to variant caller.
  # TODO2: Resolve bgzf.c collisions between vcflib and bamtools
  ${ION_VCFLIB_DIR}/Variant.cpp
  ${ION_VCFLIB_DIR}/split.cpp
  ${ION_VCFLIB_DIR}/tabixpp/tabix.cpp
  ${ION_VCFLIB_DIR}/tabixpp/index.c
  ${ION_VCFLIB_DIR}/tabixpp/bgzf.c
  ${ION_VCFLIB_DIR}/smithwaterman/LeftAlign.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/Repeats.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/IndelAllele.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/SmithWatermanGotoh.cpp
  ${ION_FREEBAYES_DIR}/src/AlleleParser.cpp

  BaseCaller/PIDloop.cpp
  BaseCaller/DPTreephaser.cpp
  BaseCaller/TreephaserSSE.cpp
  Calibration/LinearCalibrationModel.cpp
  Calibration/FlowAlignment.cpp
  Util/OptArgs.cpp
  realignment/Realigner.cpp
  file-io/ion_util.c
  file-io/ion_error.c
  ${ION_JSONCPP_DIR}/jsoncpp.cpp
  ${PROJECT_BINARY_DIR}/IonVersion.cpp
)

target_link_libraries(tvc ${ION_BAMTOOLS_LIBS} z pthread blas lapack)


add_executable(tvcassembly
  VariantCaller/IndelAssembly/IndelAssembly.cpp
  VariantCaller/TargetsManager.cpp
  VariantCaller/SampleManager.cpp

  # TODO: Actually build vcflib as a static library and link to variant caller.
  # TODO2: Resolve bgzf.c collisions between vcflib and bamtools
  ${ION_VCFLIB_DIR}/Variant.cpp
  ${ION_VCFLIB_DIR}/split.cpp
  ${ION_VCFLIB_DIR}/tabixpp/tabix.cpp
  ${ION_VCFLIB_DIR}/tabixpp/index.c
  ${ION_VCFLIB_DIR}/tabixpp/bgzf.c
  ${ION_VCFLIB_DIR}/smithwaterman/LeftAlign.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/Repeats.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/IndelAllele.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/SmithWatermanGotoh.cpp

  Util/OptArgs.cpp
  ${ION_JSONCPP_DIR}/jsoncpp.cpp
  ${PROJECT_BINARY_DIR}/IonVersion.cpp
)

target_link_libraries(tvcassembly ${ION_BAMTOOLS_LIBS} z pthread)


add_executable(tvcutils
  VariantCaller/tvcutils/tvcutils.cpp
  VariantCaller/tvcutils/prepare_hotspots.cpp
  VariantCaller/tvcutils/validate_bed.cpp
  VariantCaller/tvcutils/unify_vcf.cpp
  VariantCaller/TargetsManager.cpp
  Util/OptArgs.cpp
  ${ION_VCFLIB_DIR}/Variant.cpp
  ${ION_VCFLIB_DIR}/split.cpp
  ${ION_VCFLIB_DIR}/tabixpp/tabix.cpp
  ${ION_VCFLIB_DIR}/tabixpp/index.c
  ${ION_VCFLIB_DIR}/tabixpp/bgzf.c
  ${ION_VCFLIB_DIR}/smithwaterman/LeftAlign.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/Repeats.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/IndelAllele.cpp
  ${ION_VCFLIB_DIR}/smithwaterman/SmithWatermanGotoh.cpp
  ${ION_JSONCPP_DIR}/jsoncpp.cpp
  ${PROJECT_BINARY_DIR}/IonVersion.cpp
)

target_link_libraries(tvcutils ${ION_BAMTOOLS_LIBS} z)

install(TARGETS   tvc                                                 DESTINATION bin)
install(TARGETS   tvcassembly                                         DESTINATION bin)
install(TARGETS   tvcutils                                            DESTINATION bin)
install(PROGRAMS  bin/variant_caller_pipeline.py                      DESTINATION bin)
install(DIRECTORY share/TVC/pluginMedia                               DESTINATION share/TVC)
install(DIRECTORY share/TVC/sse                                       DESTINATION share/TVC)
install(DIRECTORY share/TVC/examples                                  DESTINATION share/TVC)


add_test(NAME    tvc_call
         COMMAND tvc
)

if(0)

add_test(tvcutils_call
   tvcutils
)

add_test(tvcutils_prepare_hotspots_call
   tvcutils prepare_hotspots
)

add_test(tvcutils_validate_bed_call
   tvcutils validate_bed
)

add_test(tvcutils_simple
   vcftools
   --vcf ./all.merged.vcf
   --bed ./effective_regions.bed
   --out ./all
   --recode
  --keep-INFO-all
)
add_test(tvc_simple
   tvc
   --output-dir  ${PROJECT_BINARY_DIR}/Testing/tvc_simple
   --output-vcf  small_variants.vcf
   --reference   ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/reference.fasta
   --input-bam   ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test.bam
   --target-file ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test_unmerged_detail.bed
   --trim-ampliseq-primers on
   --num-threads 12
)

add_test(
   variant_caller_pipeline.py
    --input-bam       ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test.bam
    --reference-fasta ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/reference.fasta
    --region-bed      ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test_merged_plain.bed
    --primer-trim-bed ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test_unmerged_detail.bed
    --output-dir      ${PROJECT_BINARY_DIR}/Testing/variant_caller_pipeline_call
)
endif()


    set(test_rm_files)
    set(test_file_prefix "AAAA")
    set(test_diff_prog  "${PROJECT_SOURCE_DIR}/Testing/diffVCF.pl")
    set(delta            0.0001)

    set(test_name        tvc_call2)
    set(test_prog        "${PROJECT_BINARY_DIR}/tvc")
    set(test_cmp_file    "${PROJECT_BINARY_DIR}/Testing/${test_name}/small_variants.vcf")
    set(test_std_file    "${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/expected/small_variants.vcf")

    set(test_args 
   --output-dir  ${PROJECT_BINARY_DIR}/Testing/${test_name}
   --output-vcf  small_variants.vcf
   --reference   ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/reference.fasta
   --input-bam   ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test.bam
   --target-file ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test_unmerged_detail.bed
   --trim-ampliseq-primers on
   --num-threads 12)

   add_test(NAME ${test_name} COMMAND ${CMAKE_COMMAND} -V -VV
      "-Dtest_name:STRING=${test_name}"
      "-Dtest_prog:STRING=${test_prog}"
      "-Dtest_prog_args:STRING=${test_args}"
      "-Dfile_cmp:FILEPATH=${test_cmp_file}"
      "-Dfile_std:FILEPATH=${test_std_file}"
      "-Drm_files:STRING=${test_rm_files}"
      "-Ddiff_prog:FILEPATH=${test_diff_prog}"
      "-Ddelta:FILEPATH=${delta}"
      -P "${PROJECT_SOURCE_DIR}/Testing/RunDiffRemoveTest.cmake"
      )


if (1)

    set(test_name        variant_caller_pipeline_call)
    set(test_prog        variant_caller_pipeline.py)
    set(test_cmp_file    "${PROJECT_BINARY_DIR}/Testing/${test_name}/TSVC_variants.vcf")
    set(test_std_file    "${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/expected/TSVC_variants.vcf")

    set(test_args 
    --input-bam       ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test.bam
    --reference-fasta ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/reference.fasta
    --region-bed      ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test_merged_plain.bed
    --primer-trim-bed ${PROJECT_SOURCE_DIR}/share/TVC/examples/example1/test_unmerged_detail.bed
    --output-dir      ${PROJECT_BINARY_DIR}/Testing/variant_caller_pipeline_call)

   add_test(NAME ${test_name} COMMAND ${CMAKE_COMMAND} -V -VV
      "-Dtest_name:STRING=${test_name}"
      "-Dtest_prog:STRING=${test_prog}"
      "-Dtest_prog_args:STRING=${test_args}"
      "-Dfile_cmp:FILEPATH=${test_cmp_file}"
      "-Dfile_std:FILEPATH=${test_std_file}"
      "-Drm_files:STRING=${test_rm_files}"
      "-Ddiff_prog:FILEPATH=${test_diff_prog}"
      "-Ddelta:FILEPATH=${delta}"
      -P "${PROJECT_SOURCE_DIR}/Testing/RunDiffRemoveTest.cmake"
      )

endif()


if (1)

    set(test_name        Z06-506-CCP)
    set(test_prog        variant_caller_pipeline.py)

    set(REF                 /mnt/TS/source/hg19/hg19.fasta)
    set(BED_UNMERGED_DETAIL /mnt/TS/source/tvc_test_Z06-506-CCP/unmerged_detail_CCP.20131001.designed.bed)
    set(BED_MERGED_PLAIN    /mnt/TS/source/tvc_test_Z06-506-CCP/merged_plain_CCP.20131001.designed.bed)
    set(BAM                 /mnt/TS/source/tvc_test_Z06-506-CCP/IonXpress_019_rawlib.bam)
    set(TVC_PARAM           /mnt/TS/source/tvc_test_Z06-506-CCP/local_parameters.json)
    set(HOTSPOT             /mnt/TS/source/tvc_test_Z06-506-CCP/hotspot.vcf)
    set(test_std_file       /mnt/TS/source/tvc_test_Z06-506-CCP/expected/TSVC_variants.vcf)
    set(test_cmp_file       "${PROJECT_BINARY_DIR}/Testing/${test_name}/TSVC_variants.vcf")
    set(TMP_DIR             "${PROJECT_BINARY_DIR}/Testing/${test_name}")

    set(test_args 
    --parameters-file   ${TVC_PARAM}
    --input-bam         ${BAM}
    --reference-fasta   ${REF}
    --region-bed        ${BED_MERGED_PLAIN}
    --primer-trim-bed   ${BED_UNMERGED_DETAIL}
    --hotspot-vcf       ${HOTSPOT}
    --postprocessed-bam ${TMP_DIR}/trimmed.bam
    --output-dir        ${TMP_DIR})

   add_test(NAME ${test_name} COMMAND ${CMAKE_COMMAND} -V -VV
      "-Dtest_name:STRING=${test_name}"
      "-Dtest_prog:STRING=${test_prog}"
      "-Dtest_prog_args:STRING=${test_args}"
      "-Dfile_cmp:FILEPATH=${test_cmp_file}"
      "-Dfile_std:FILEPATH=${test_std_file}"
      "-Drm_files:STRING=${test_rm_files}"
      "-Ddiff_prog:FILEPATH=${test_diff_prog}"
      "-Ddelta:FILEPATH=${delta}"
      -P "${PROJECT_SOURCE_DIR}/Testing/RunDiffRemoveTest.cmake"
      )

endif()
