# uf2

USB Flashing Format (UF2) for your build.zig

This package is for assembling uf2 files from ELF binaries. This format is used for flashing a microcontroller over a mass storage interface, such as the Pi Pico.

See https://github.com/microsoft/uf2#file-containers for how we're going to embed file source into the format.

For use in a build.zig:

```zig
pub fn build(b: *Build) void {
    // ...

    const uf2_dep = b.dependency("uf2", .{});

    const elf2uf2_run = b.addRunArtifact(uf2_dep.artifact("elf2uf2"));

    // family id
    elf2uf2_run.addArgs(&.{"--family-id", "RP2040"});

    // elf file
    elf2uf2_run.addArg("--elf-path");
    elf2uf2_run.addArtifactArg(exe.inner);

    // output file
    const uf2_file = elf2uf2_run.addPrefixedOutputFileArg("--output-path", "test.uf2");
    b.addInstallFile(uf2_file, "bin/test.uf2");
}
```
