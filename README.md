# Simple C++ Custom Buildpack 

An example paketo custom buildpack to compile c++ code. As the `g++` compiler is present on the base images no dependencies are required. https://github.com/paketo-buildpacks/base-stack-release/blob/1.2.1/build-receipt 


## Usage

~~~sh
mkdir ./cpp-sample
cat > "./cpp-sample/main.cpp" << EOL
    #include <iostream>
    int main()
    {
        std::cout << "Hello World\n";
        return 0;
    }
EOL

# test app
g++ -o out -I ./cpp-sample/ cpp-sample/*.cpp
./out
# should print "Hello World" 

# clone buildpack locally 
git clone git@github.com:miclip/cpp-buildpack.git

# execute build...
pack build test-cpp --path ./cpp-sample --buildpack ./cpp-buildpack --builder paketobuildpacks/builder:base

# test image
docker run test-cpp

~~~


## Use in Tanzu Build Service 

~~~sh 
# package and publish buildpack image 
pack buildpack package gcr.io/some-project/custom-buildpacks/cpp-buildpack -f image -p ./cpp-buildpack  --publish

# add to cluster store
kp clusterstore add default -b gcr.io/some-project/custom-buildpacks/cpp-buildpack:1.0.0

# create a builder 
kp builder save cpp-test -b cpp-buildpack -n builds -s base --store default --tag gcr.io/some-project/custom-builders/cpp-builder

# create image resource 
kp image save cpp-sample -n builds --git https://github.com/miclip/cpp-sample.git --git-revision master -b cpp-test --tag gcr.io/some-project/tbs-examples/cpp-sample 

# test image 
docker run gcr.io/some-project/tbs-examples/cpp-sample
Hello World

~~~
