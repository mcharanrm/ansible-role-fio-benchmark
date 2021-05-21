# ansible-role-fio-benchmark

**This ansible role is to run the `fio` benchamrk on a running instance.**

What it does ??

1. Test write throughput by performing sequential writes.
2. Test read throughput by performing sequential reads.
3. Test write IOPS by performing random writes
4. Test read IOPS by performing random reads 


## Installation

To install the role, create `requirements.yaml` with this content:

    - name: run_fio_benchmark
      src: https://github.com/mcharanrm/ansible-role-fio-benchmark.git
      version: origin/main

and then to install the role:

    ansible-galaxy install -r requirements.yaml --roles-path roles/

Now the role is available in `roles/run_fio_benchmark/`.

Check `fio_benchmark.yaml` for example on how to run the playbook.


## Configuration

**When calling the role, configure these variables:**

* `fiotest_directory` - Directory path to store `fio` benchmark data
   - variable type: string, absolute path to the directory

* `fiotest_rounds` - Number of times to repeat the `fio` benchmark
  - variable type: integer

* `fiotest_rw_throughput` - Job name and operation to perform
  - variable type: list of dictionaries.
  -- `name`: Name the `fio` job
  -- `rw`: Specify the required operation to perform on the `fio` job, read,write,randread,write, etc...

* `fiotest_rw_throughput_numjobs` - Number of parallel `fio` jobs to run
  - variable type: integer

* `fiotest_rw_throughput_jobsize` - Size of a `fio` job
   - variable type: string

* `fiotest_rw_throughput_runtime` - Tell `fio` to terminate processing after the specified period of time
   - variable type: string
   - **warning:** Total amount of disk space consumed by the test would be greatly influenced by the `fiotest_rw_throughput_numjobs` and `fiotest_rw_throughput_jobsize`

* `fiotest_rw_throughput_block_size` - In how large chunks are we issuing I/O
   - variable type: string

* `fiotest_rw_throughput_iodepth` - How many IOs it issues to the OS at any given time
   - variable type: integer


### Default variables

```
# fio benchmark parameters for write/read throughput and write/read iops of a disk on running instance
fiotest_directory: '/root/fiotest'
fiotest_rounds: 5

fiotest_rw_throughput:
- name: 'write_throughput'
  rw: 'write'
- name: 'read_throughput'
  rw: 'read'
fiotest_rw_throughput_numjobs: 8
fiotest_rw_throughput_jobsize: '1G'
fiotest_rw_throughput_runtime: '60s'
fiotest_rw_throughput_block_size: '1M'
fiotest_rw_throughput_iodepth: 64

fiotest_rw_iops:
- name: 'write_iops'
  rw: 'randwrite'
- name: 'read_iops'
  rw: 'randread'
fiotest_rw_iops_numjobs: 3
fiotest_rw_iops_jobsize: '10G'
fiotest_rw_iops_runtime: '60s'
fiotest_rw_iops_block_size: '4K'
fiotest_rw_iops_iodepth: 64
```


## References

1. To read more about fio benchmark
  - https://fio.readthedocs.io/en/latest/fio_doc.html

2. This ansible role is greatly influenced by the google cloud benchmarking-physical-disk performance article
  - https://cloud.google.com/compute/docs/disks/benchmarking-pd-performance
