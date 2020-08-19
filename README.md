# VMPAttack
![alt text](https://raw.githubusercontent.com/0xnobody/vmpattack/master/screenshot.png "VMPAttack")
 A Work-In-Progress VMP to VTIL lifter.
 Works for VMProtect 3.X x64.

## Usage
 Literally drag + drop the unpacked victim file onto VMPAttack.exe.
 Lifted VTIL routines will appear in a folder named "VMPAttack-Output".

## Advanced Usage
 All lifting functionality depends on the `vmpattack` root class object. This object can easily be constructed using a byte vector of the target image.
 You can lift any routine manually by passing the VMEntry **RVA** and entry stub value in a `lifting_job` structure to the `vmpattack::lift` function.

 ![alt text](https://raw.githubusercontent.com/0xnobody/vmpattack/master/entry_stub.png "Entry Stub")

 `lifting_job`s can be automatically generated by providing the **RVA** of the entry stub (see above) to the `vmpattack::analyze_entry_stub` function.

 Example usage:
 ```C++
    std::vector<uint8_t> buffer = read_file( file_path );

    vmpattack instance( buffer );

    if ( auto result = instance.analyze_entry_stub( my_rva ) )
    {
        if ( auto routine = instance.lift( result->job ) )
        {
            vtil::optimizer::apply_all_profiled( *routine );
            vtil::save_routine( *routine, "C:\\my_routine.vtil" );
        }
    }
```

## Building
 Building in VS is as simple as replacing the include/library directories to VTIL/Keystone/Capstone in the vcxproj.
 
 The project now also universally supports CMake and platforms other than Windows.
 
 The project requires C++20.

## Issues
 Stability is the main issue. Sometimes the lifter or optimizer can hang unexpectedly, or fail to lift certain branches.
 The lifter also does not currently handle switch tables.

## Licence
 Licensed under the GPL-3.0 License. No warranty is provided of any kind.
