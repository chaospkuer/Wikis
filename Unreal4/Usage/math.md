










# random #
```c++
// random r: r >= min and r <= max
FMath::RandRange(int32 min, int32 max);
FMath::RandRange(float min, float max);
```


# clamp #
```c++
// if value < min then value = min
// if value > max then value = max
FMath::Clamp(float value, float min, float max);

// use template
float zoomFector = 2.0f;
FMath::Clamp<float>(zoomFector, 0.0f, 1.0f);
```

# lerp #
```c++
float zoomFector = 2.0f;
zoomFector = FMath::Clamp<float>(zoomFector, 0.0f, 1.0f);
// lerp factor must be constrained between 0, 1
FMath::Lerp<float>(90.0f, 60.0f, zoomFector);
```

