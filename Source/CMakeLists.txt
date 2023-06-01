cmake_minimum_required(VERSION 3.0)

#include some macros from another file...
#INCLUDE(${CMAKE_SOURCE_DIR}/IJMacros.txt)

#Change PROJECT_NAME to the name of your project
PROJECT(LiveWireImageFunctionTest)

#The following lines are required to use Dart
ENABLE_TESTING()
INCLUDE(Dart)
FIND_PROGRAM(IMAGE_COMPARE ImageCompare)

find_package(ITK REQUIRED)
include(${ITK_USE_FILE})

IF(BUILD_TESTING)

SET(CurrentExe
    "itkLiveWireImageFunctionTest"
    )
ADD_EXECUTABLE(${CurrentExe}
               ${CurrentExe}.cxx
               itkLiveWireImageFunction.h
               itkLiveWireImageFunction.txx
           )
TARGET_LINK_LIBRARIES(${CurrentExe}
                      ${ITK_LIBRARIES})

ENDIF(BUILD_TESTING)


#the following line is an example of how to add a test to your project.

#Testname is the title for this particular test.  ExecutableToRun is the

#program which will be running this test.  It can either be a part of this

#project or an external executable.  After that list any args that are needed

#for this test.  Include as many tests as you like.  If your project doesn't have

#any tests you can comment out or delete the following line.

# ADD_TEST(Testname ExecutableToRun arg1 arg2 arg3)


ADD_TEST( BrainLiveWireTest ${CurrentExe} 
          ${CMAKE_SOURCE_DIR}/Images/BrainProtonDensitySlice256x256.nii 
          ${CMAKE_BINARY_DIR}/outputBrain.nii.gz 94 29 92 231 
        ) 


ADD_TEST( BrainImageCompare ${IMAGE_COMPARE} 
          ${CMAKE_BINARY_DIR}/outputBrain.nii.gz 
          ${CMAKE_SOURCE_DIR}/Images/outputBrainProtonDensitySlice256x256.nii.gz
        ) 

ADD_TEST( BinaryLiveWireTest ${CurrentExe} 
          ${CMAKE_SOURCE_DIR}/Images/BinaryImage.nii 
          ${CMAKE_BINARY_DIR}/outputBinary.nii.gz 24 113 161 39 
          ${CMAKE_SOURCE_DIR}/Images/maskBinaryImage.nii.gz
        ) 


ADD_TEST( BinaryImageCompare ${IMAGE_COMPARE} 
          ${CMAKE_BINARY_DIR}/outputBinary.nii.gz
          ${CMAKE_SOURCE_DIR}/Images/outputBinaryImage.nii.gz
        ) 

