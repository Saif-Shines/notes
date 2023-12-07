# Typescript

# Hello TypeScript

[Courses](https://www.typescript-training.com/)

[GitHub - mike-north/ts-fundamentals-v3: Mike North's TypeScript Fundamentals & Intermediate TypeScript courses](https://github.com/mike-north/ts-fundamentals-v3/tree/main)

### Objects

```tsx
const phones: {
    [k: string]: {
        country: string,
        area: string,
        number: string
    } | undefined
} = {}

const phone = {
    office: {
        country: "India",
        area: "Tenali Rural",
        number: "9038403948"
    },
    home: {
        country: "India",
        area: "Tenali Rural",
        number: "9038403948"
    }
}
```

### tuples

```tsx
const numPair: [number, number] = [4, 5];

numPair.push(4);
numPair.pop();
```