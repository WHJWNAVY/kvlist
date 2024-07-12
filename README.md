# kvlist
> A key-value list library from openwrt libubox.

## usage
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <stdbool.h>
#include <string.h>
#include <limits.h>
#include <inttypes.h>

#include <stdarg.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <syslog.h>
#include <unistd.h>

#include "lib/kvlist.h"

#define CH_LOG(LEVEL, FMT, ...)                                                  \
    do {                                                                         \
        fprintf(stderr, "(%s:%d) " FMT "\n", __func__, __LINE__, ##__VA_ARGS__); \
    } while (0)

#define CH_DEBUG(FMT, ...) CH_LOG(LOG_DEBUG, FMT, ##__VA_ARGS__)
#define CH_INFO(FMT, ...) CH_LOG(LOG_INFO, FMT, ##__VA_ARGS__)
#define CH_WARN(FMT, ...) CH_LOG(LOG_WARNING, FMT, ##__VA_ARGS__)
#define CH_ERROR(FMT, ...) CH_LOG(LOG_ERR, FMT, ##__VA_ARGS__)

static int32_t test_int_get_len(struct kvlist *kv, const void *data) {
    return sizeof(uint32_t);
}

#define test_str_get_len kvlist_strlen

int main(int argc, char **argv) {
    char *k = NULL;
    char *sv = NULL;
    uint32_t iv = 0;
    uint32_t *ip = 0;

    CH_DEBUG("TEST KVLIST STR");
    KVLIST(test_str, test_str_get_len);

    k = "str1";
    sv = "data1";
    kvlist_set(&test_str, k, sv);
    k = "str2";
    sv = "data2";
    kvlist_set(&test_str, k, sv);
    k = "str3";
    sv = "data3";
    kvlist_set(&test_str, k, sv);
    k = "str4";
    sv = "data4";
    kvlist_set(&test_str, k, sv);

    k = "str1";
    sv = kvlist_get(&test_str, k);
    CH_INFO("test_str k[%s], v[%s]", k, sv);
    k = "str2";
    sv = kvlist_get(&test_str, k);
    CH_INFO("test_str k[%s], v[%s]", k, sv);
    k = "str3";
    sv = kvlist_get(&test_str, k);
    CH_INFO("test_str k[%s], v[%s]", k, sv);
    k = "str4";
    sv = kvlist_get(&test_str, k);
    CH_INFO("test_str k[%s], v[%s]", k, sv);

    kvlist_free(&test_str);

    CH_DEBUG("TEST KVLIST INT");
    KVLIST(test_int, test_int_get_len);

    k = "int1";
    iv = 100;
    kvlist_set(&test_int, k, &iv);
    k = "int2";
    iv = 200;
    kvlist_set(&test_int, k, &iv);
    k = "int3";
    iv = 300;
    kvlist_set(&test_int, k, &iv);
    k = "int4";
    iv = 400;
    kvlist_set(&test_int, k, &iv);

    kvlist_for_each(&test_int, k, ip) {
        CH_INFO("test_int k[%s], v[%u]", k, *ip);
    }

    kvlist_free(&test_int);

    return 0;
}
```

## output
```
$ make
$ ./kvlist
(main:41) TEST KVLIST STR
(main:59) test_str k[str1], v[data1]
(main:62) test_str k[str2], v[data2]
(main:65) test_str k[str3], v[data3]
(main:68) test_str k[str4], v[data4]
(main:72) TEST KVLIST INT
(main:89) test_int k[int1], v[100]
(main:89) test_int k[int2], v[200]
(main:89) test_int k[int3], v[300]
(main:89) test_int k[int4], v[400]
```
