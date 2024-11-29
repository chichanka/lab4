#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void read_file_standard(const char *filename) {
    FILE *file = fopen(filename, "rb");
    if (file == NULL) {
        perror("Failed to open file");
        return;
    }

fseek(file, 0, SEEK_END);
    long filesize = ftell(file);
    fseek(file, 0, SEEK_SET);

char *buffer = (char *)malloc(filesize);
    if (buffer == NULL) {
        perror("Failed to allocate memory");
        fclose(file);
        return;
    }

clock_t start = clock();
    fread(buffer, 1, filesize, file);
    clock_t end = clock();
    printf("Standard IO read time: %.2f seconds\n", (double)(end - start) / CLOCKS_PER_SEC);

free(buffer);
    fclose(file);
}

int main() {
    const char *filename = "largefile.dat";
    read_file_standard(filename);
    return 0;
}
