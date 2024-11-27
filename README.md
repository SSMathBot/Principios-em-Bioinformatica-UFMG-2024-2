# Detecção de Wolbachia em Bibliotecas de Pequenos RNAs

O objetivo deste projeto é orientar os alunos a efetuar o passo a passo prático na detecção da
 bactéria Wolbachia em bibliotecas de pequenos RNAs.

Esta busca é de extrema importância para a compreensão e estudos do potencial desta bactéria em aplicações
 biotecnológicas, especialmente quando pensamos em seu recente uso para fins de saúde pública no combate à
 arboviroses.

PASSO A PASSO:

1- Download de dados SRA

substitua os "XX" pelo restante dos números do nome do seu arquivo

comando: fasterq-dump SRRXXXXXXX

2- Avaliação da qualidade de leitura com Phred

substitua os "XX" pelo restante dos números do nome do seu arquivo

comando: fastqc SRRXXXXXXX.fastq

3- Limpeza e corte de sequências de baixa qualidade com Trim Galore

substitua os valores de exemplo nas flags pelos valores que são desejáveis ao seu objetivo

comando: trim_galore --fastqc -q 25 --trim-n --max_n 0 -j 1 --length 18 --dont_gzip SRRXXXXXXX.fastq

4- Baixe uma sequência de referência

exemplo: efetch -db nucleotide -id CP031221.1 -format fasta > wAlbB_CP031221.1.fasta

5- Construção do índice de alinhamento com Bowtie

comando: bowtie-build reference.fasta reference_index

6- Alinhamento das sequências

substitua os valores das flags pelos valores que são desejáveis para o seu objetivo

comando: bowtie -f -S -a -v 0 -p 3 -t reference_index infected_SRR12893566.fasta >
 infected_SRR12893566.sam 2> infected_SRR12893566_bowtie.log

7- Filtragem dos alinhamentos

comando: samtools view -S -h -F 4 infected_SRR12893566.sam > infected_SRR12893566_onlymapped.sam

8- Conatgem de leituras alinhadas

comando: awk '$1 !~ /^@/ {print $3}' infected_SRR12893566_onlymapped.sam | sort | uniq -c

9- Visualização do Heatmap


