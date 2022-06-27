# Simple C++ Custom Buildpack 

An example paketo custom buildpack to compile c++ code. 

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
