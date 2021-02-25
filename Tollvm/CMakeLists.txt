add_llvm_library(Tollvm MODULE
    # List your source files here.
    BogusControlFlow.cpp
    CryptoUtils.cpp
    Flattening.cpp
    SplitBasicBlocks.cpp
    Substitution.cpp
    Utils.cpp
    Enter.cpp

    ADDITIONAL_HEADER_DIRS
    ${LLVM_MAIN_INCLUDE_DIR}/llvm/Transforms

    DEPENDS
    intrinsics_gen
)




# Use C++11 to compile your pass (i.e., supply -std=c++11).
target_compile_features(Tollvm PRIVATE cxx_range_for cxx_auto_type)

include_directories(include)

# LLVM is (typically) built with no C++ RTTI. We need to match that;
# otherwise, we'll get linker errors about missing RTTI data.
set_target_properties(Tollvm PROPERTIES
    COMPILE_FLAGS "-fno-rtti"
)

# Get proper shared-library behavior (where symbols are not necessarily
# resolved when the shared library is linked) on OS X.
if(APPLE)
    set_target_properties(Tollvm PROPERTIES
        LINK_FLAGS "-undefined dynamic_lookup"
    )
endif(APPLE)
