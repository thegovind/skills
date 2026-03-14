# Embeds Reference

## Embed Notes

```markdown
![[Note Name]]                         Full note
![[Note Name#Heading]]                 Section under heading
![[Note Name#^block-id]]               Specific block
```

## Embed Images

```markdown
![[image.png]]                         Full size
![[image.png|300]]                     Width only (maintains ratio)
![[image.png|640x480]]                 Width x Height
```

## External Images

```markdown
![Alt text](https://example.com/image.png)
![Alt text|300](https://example.com/image.png)
```

## Embed Audio

```markdown
![[audio.mp3]]
![[audio.ogg]]
```

Supported formats: mp3, webm, wav, m4a, ogg, 3gp, flac.

## Embed Video

```markdown
![[video.mp4]]
![[video.webm]]
```

Supported formats: mp4, webm, ogv.

## Embed PDF

```markdown
![[document.pdf]]
![[document.pdf#page=3]]
![[document.pdf#height=400]]
```

## Embed Lists

The list must have a block ID:

```markdown
- Item 1
- Item 2
- Item 3

^list-id
```

Then embed: `![[Note#^list-id]]`

## Embed Search Results

````markdown
```query
tag:#project status:done
```
````
