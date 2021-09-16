---
description: 好用的公共数据下载工具
---

# 下载公共测序数据的另一种姿势（kingfisher）

### 写在前面

* 一般在进行公共测序数据挖掘的时候，需要从公共数据库中（SRA、ENA、DDBJ等）下载自己所需的测序数据。下载数据时，往往会遇到网速限制或下载链接不可用等因素，当某个数据库的目标数据下载不来时，可以去其他公共数据库下载，因为这三者的数据是共享的。

* 问题来了，手动在不同的数据库中检索与下载目标数据确实较为繁琐。这时可以试试使用`Kingfisher`来自动下载数据。

![](.gitbook/assets/image.png)

### Kingfisher安装与使用

安装

```text
conda create -c conda-forge -c bioconda -n kingfisher pigz python extern curl sra-tools pandas requests aria2
conda activate kingfisher
#使用conda activate不能成功激活环境时可以尝试使用：source activate kingfisher
pip install bird_tool_utils'>='0.2.17
git clone https://github.com/wwood/kingfisher-download
cd kingfisher-download/bin
export PATH=$PWD:$PATH
kingfisher -h
#弹出帮助文档即安装成功
```

下载数据

**注意：如果只想下载某个确定的SRA数据，则使用-r参数，提供SRR Number即可，如SRR12042866；若是想批量下载某个BioProject中的所有数据，则可以使用-p参数，提供BioProject Number，如PRJNA640275或SRP267791。**

```text
kingfisher get -r SRP267791 -m ena-ascp  ena-ftp prefetch aws-http
#-r Run number(s) to download/extract e.g. ERR1739691
#-p BioProject IDs number(s) to download/extract from e.g. PRJNA621514 or SRP260223
# -m ena-ascp、ena-ftp、prefetch、aws-http、aws-cp、gcp-cp
# --download-threads 线程数
```

数据下载源介绍（-m参数）

> ena-ascp,调用Aspera从ENA中下载.fastq.gz数据  
> ena-ftp，调用curl从ENA中下载.fastq.gz数据  
> prefetch，调用prefetch从NCBI SRA数据库中下载SRA数据，然后默认使用fasterq-dump对其进行拆分转换  
> aws-http，调用aria2c从AWS Open Data Program中下载SRA数据，然后默认使用fasterq-dump对其进行拆分转换  
> **也就是说，如果是用的ENA源 直接下载的就是fastq，如果用的SRA或其他，那就是下载SRA数据 然后kingfisher再自动调用fasterq-dump转换成fastq**

![](.gitbook/assets/image%20%281%29.png)

SRA格式转换成fastq格式，调用fasterq-dump

```text
kingfisher extract --sra SRR1574780.sra -t 20 -f fastq.gz
#-f,指定转换输出的文件格式，支持fastq,fastq.gz,fasta,fasta.gz
#-t，指定线程数
```

### 写在最后

大家加油，有空找我干饭。

