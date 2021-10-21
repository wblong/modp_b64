# Base64

## Demo

```cpp

#include <modp_b64.h>
#include <filesystem>
#include <vector>
#include <gsl/span>

namespace {
    std::vector<std::byte> decodeBase64(gsl::span<const std::byte> data) {

            std::vector<std::byte> result(modp_b64_decode_len(data.size()));

            size_t resultLength = modp_b64_decode(
                reinterpret_cast<char*>(result.data()),
                reinterpret_cast<const char*>(data.data()),
                data.size()
            );

            if (resultLength == size_t(-1)) {
                result.clear();
                result.shrink_to_fit();
            }
            else {
                result.resize(resultLength);
            }

            return result;
    }
}

int main(){

    const std::filesystem::path ctm_file = TestPath / "ctm";
    std::vector<std::byte> ctm_encode_data = readFile(ctm_file);
    gsl::span<std::byte>tmp_data(reinterpret_cast<std::byte*>(ctm_encode_data.data()), ctm_encode_data.size());
    
    std::vector<std::byte>ctm_decode_data= decodeBase64(tmp_data);
    const std::filesystem::path ctm_file_decode = TestPath / "ctm_decode";
    gsl::span < std::byte> ctm (reinterpret_cast<std::byte*>(ctm_file_decode.data()), ctm_file_decode.size());
    writeFile(ctm_file_decode, ctm);

    return 0;
}

```

## Usage

```
//Root.CMakeLists.txt
add_subdirectory(modp_b64)
install(TARGETS modp_b64)
```
```
//Demo.CMakeLists.txt
target_link_libraries(Demo
    PUBLIC
    PRIVATE
        modp_b64
)
```
