```zsh

cd /Users/simon/Documents/SimonYu/Work/KS_GBMI/01_LDSC/02_ldsc
source activate ldsc

python ldsc.py -h
python munge_sumstats.py -h

# wget https://data.broadinstitute.org/alkesgroup/LDSCORE/w_hm3.snplist.bz2
# wget https://data.broadinstitute.org/alkesgroup/LDSCORE/eur_w_ld_chr.tar.bz2

# tar -jxvf eur_w_ld_chr.tar.bz2
# bunzip2 w_hm3.snplist.bz2


# KS data munge
python munge_sumstats.py \
    --sumstats /Users/simon/Documents/SimonYu/Work/Gwas_data/Calculi/KS_BBJ.txt \
    --N 179000 \
    --out /Users/simon/Documents/SimonYu/Work/Gwas_data/Calculi/Calculi_sumstats/KS_BBJ 

python munge_sumstats.py \
    --sumstats /Volumes/ST/Gwas_data/Calculi/merged_data/KS_BBJ.txt \
    --N 178726 \
    --merge-alleles w_hm3.snplist \
    --out /Volumes/ST/Gwas_data/Calculi/Calculi_sumstats/KS_BBJ1


# GBMI data munge

python munge_sumstats.py \
    --sumstats /Users/simon/Documents/SimonYu/Work/Gwas_data/GBMI_EAS/Gout_GBMI_EAS.txt \
    --N 349359 \
    --out /Users/simon/Documents/SimonYu/Work/Gwas_data/GBMI_EAS/GBMI_EAS_sumstats/Gout_GBMI_EAS \

python munge_sumstats.py \
    --sumstats /Users/simon/Documents/SimonYu/Work/Gwas_data/GBMI_EAS/Stroke_GBMI_EAS.txt \
    --N 268930 \
    --out /Users/simon/Documents/SimonYu/Work/Gwas_data/GBMI_EAS/GBMI_EAS_sumstats/Stroke_GBMI_EAS \


#LD Score Regression 


for gbmi in Stroke_GBMI_EAS Gout_GBMI_EAS;
do
    python ldsc.py \
        --rg /Users/simon/Documents/SimonYu/Work/Gwas_data/GBMI_EAS/GBMI_EAS_sumstats/"$gbmi".sumstats.gz,/Users/simon/Documents/SimonYu/Work/Gwas_data/Calculi/Calculi_sumstats/KS_BBJ.sumstats.gz \
        --ref-ld-chr eas_ldscores/ \
        --w-ld-chr eas_ldscores/ \
        --out ./result/"$gbmi"_KS_BBJ
done

for gbmi in Stroke_GBMI_EAS Gout_GBMI_EAS;
do
    python ldsc.py \
        --rg /Users/simon/Documents/SimonYu/Work/Gwas_data/Calculi/Calculi_sumstats/KS_BBJ.sumstats.gz,/Users/simon/Documents/SimonYu/Work/Gwas_data/GBMI_EAS/GBMI_EAS_sumstats/"$gbmi".sumstats.gz \
        --ref-ld-chr eas_ldscores/ \
        --w-ld-chr eas_ldscores/ \
        --out ./result/KS_BBJ_"$gbmi"
done

python ldsc.py \
        --rg /Volumes/ST/Gwas_data/Calculi/Calculi_sumstats/KS_BBJ1.sumstats.gz,/Volumes/ST/Gwas_data/GBMI_EAS/GBMI_EAS_sumstats/Gout_GBMI_EAS.sumstats.gz \
        --ref-ld-chr eas_ldscores/ \
        --w-ld-chr eas_ldscores/ \
        --out /Users/simon/Documents/01MedWork/2210_KS_GBMI/01_LDSC/03_result/Gout_KS_BBJ1