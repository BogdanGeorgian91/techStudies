December 2021

I needed to reset my M1 NVRAM for reasons, here to save you some seconds of googling

```r
sudo nvram -c
```

You might see some errors regarding some services that cannot be restarted (fmm-mobileme and fmm-computer-name), but you can ignore those, afterwards restart your computer and it should be fine