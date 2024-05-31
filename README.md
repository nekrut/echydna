# echydna

Standalone python module to generate and operate on random DNA strings.

# Example Usage

Generate a pair of 10Kbp sequences with expected 2% divergence, writing them to
separate files.
```bash  
    from random  import seed as random_seed,randint
    from echydna import EchyDna
    EchyDna.background = "AAACCGGTTT"

    itemLen = 10000

    apple  = EchyDna(itemLen)
    orange = apple.mutate(subRate=0.02)
    apple.fasta ("apple.fa" ,name="APPLE" ,wrap=100)
    orange.fasta("orange.fa",name="ORANGE",wrap=100)
```

Generate a pair of sequences with three differently diverged 10Kbp segments,
separated by 1Kbp flanks, with rearrangements and with one segment
reverse-complemented.
```bash  
    from random  import seed as random_seed,randint
    from echydna import EchyDna
    EchyDna.background = "AAACCGGTTT"

    itemLen  = 10000
    flankLen = 1000
    divergences = [0.02,0.10,0.20]
    rearrangementOrder = [2,0,1]
    complementIx = 1

    apples  = [EchyDna(itemLen) for _ in range(3)]
    oranges = [apples[appleIx].mutate(subRate=divergence) for (appleIx,divergence) in enumerate(divergences)]
    oranges[complementIx] = -oranges[complementIx]  # reverse-complement

    bigApple  = EchyDna(flankLen)
    bigOrange = EchyDna(flankLen)
    for (apple,orange) in zip(apples,oranges):
        bigApple  += apple  + EchyDna(flankLen)
        bigOrange += orange + EchyDna(flankLen)

    bigApple.fasta ("apple.fa" ,name="APPLE" ,wrap=100)
    bigOrange.fasta("orange.fa",name="ORANGE",wrap=100)
```
