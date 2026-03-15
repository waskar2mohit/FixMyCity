# Image Upload Guidelines

Your app now validates images to ensure they show actual civic infrastructure issues. Here's what you need to know:

## ✅ Acceptable Images

Upload **clear photos** showing:

### 🕳️ Potholes
- Holes in road pavement
- Cracks in asphalt
- Damaged road surfaces
- Broken pavement

### 💧 Water Issues
- Water leaks
- Flooding on streets
- Drainage problems
- Broken water pipes
- Puddles from leaks

### 🗑️ Trash/Garbage
- Overflowing trash bins
- Illegal dumping
- Litter on streets
- Garbage piles
- Debris

### ⚡ Electrical Issues
- Broken streetlights
- Exposed electrical wires
- Damaged utility poles
- Non-working traffic lights

### 🛣️ Road Damage
- Damaged road signs
- Broken barriers/guardrails
- Damaged sidewalks
- Road construction issues
- Street damage

### 📋 Other Civic Issues
- Any other municipal infrastructure problems
- Public property damage
- Street maintenance issues

## ❌ Images That Will Be Rejected

The AI will **automatically reject** images showing:

### Personal Content
- ❌ Selfies or portraits
- ❌ People's faces
- ❌ Personal photos
- ❌ Food or meals
- ❌ Pets or animals

### Indoor/Private Spaces
- ❌ Building interiors
- ❌ Rooms (bedrooms, offices)
- ❌ Restaurants or kitchens
- ❌ Private property (unless showing civic issue)

### Non-Civic Objects
- ❌ Screenshots or computer screens
- ❌ Websites or apps
- ❌ Documents or papers
- ❌ Vehicles (cars, buses) unless part of the issue
- ❌ Nature scenes (landscapes, forests, beaches)
- ❌ Sky, clouds, or scenery
- ❌ Plants or gardens (unless showing municipal maintenance issue)

### Inappropriate Content
- ❌ Violence or weapons
- ❌ Explicit or sexual content
- ❌ Offensive material

## 🤖 How AI Validation Works

When you upload an image:

1. **AI Analysis** - The image is sent to Hugging Face's vision model
2. **Description Generated** - AI describes what it sees
3. **Category Detection** - Matches description to civic issue categories
4. **Validation** - Checks if it's a legitimate civic issue

### What You'll See:

#### ✅ Image Approved
```
AI detected: Pothole (85% confidence)
```
→ Category auto-selected, you can submit

#### ❌ Image Rejected
```
⚠️ Image Rejected

Unable to identify a civic infrastructure issue in this image.
Please upload a clear photo showing road damage, potholes,
water leaks, trash, electrical problems, or other municipal
infrastructure issues.
```
→ Image cleared, you must upload a different photo

## 📸 Tips for Best Results

### ✅ DO:
- Take clear, well-lit photos
- Get close to the issue
- Focus on the problem
- Show the full extent of damage
- Use good lighting (daytime is best)

### ❌ DON'T:
- Upload blurry photos
- Take photos from too far away
- Include too much background
- Upload screenshots
- Use flash at night (if possible, use natural light)

## 🎯 Confidence Levels

The AI returns a confidence score:

| Confidence | Meaning |
|-----------|---------|
| **75-85%** | Exact match - clear civic issue detected |
| **65-75%** | Strong match - civic issue identified |
| **50-65%** | Moderate match - probably a civic issue |
| **< 50%** | **REJECTED** - Not clearly a civic issue |

## 🔍 What Happens Behind the Scenes

### Example 1: Pothole Photo ✅
```
User uploads: Photo of road with holes
↓
AI sees: "a road with potholes and cracks"
↓
Matches keywords: pothole ✅, hole ✅, road ✅
↓
Category: Pothole (85% confidence)
↓
Result: ✅ APPROVED
```

### Example 2: Screenshot ❌
```
User uploads: Screenshot of website
↓
AI sees: "web site, website, internet site"
↓
Matches keywords: website ✅ (non-civic keyword)
↓
Category: None
↓
Result: ❌ REJECTED - "not a civic infrastructure issue"
```

### Example 3: Random Photo ❌
```
User uploads: Photo of their dog
↓
AI sees: "a dog sitting on grass"
↓
Matches keywords: dog ✅, animal ✅ (non-civic keywords)
↓
Category: None
↓
Result: ❌ REJECTED - "not a civic infrastructure issue"
```

## 🛠️ For Developers

### Customizing Rejection Criteria

To adjust what gets rejected, edit the keyword lists in:
[app/api/analyze-image/route.ts](app/api/analyze-image/route.ts)

#### Non-Civic Keywords (Line ~85):
```typescript
const nonCivicKeywords = [
  'website', 'screen', 'monitor',  // Screens
  'person', 'face', 'selfie',      // People
  'food', 'meal',                  // Food
  'animal', 'dog', 'cat',          // Animals
  // Add more as needed...
]
```

#### Civic Keywords (Line ~130):
```typescript
const categoryPatterns = {
  pothole: [
    { keywords: ['pothole', 'hole'], weight: 1.0 },
    // Add more patterns...
  ]
}
```

### Adjusting Confidence Threshold

Change the minimum score required (currently 0.35):

```typescript
// Line ~203
if (!bestCategory || bestScore < 0.35) {  // ← Adjust this
  // Reject as non-civic
}
```

Lower number = More strict (rejects more)
Higher number = More lenient (accepts more)

## 🧪 Testing

### Good Test Images:
1. Upload a photo of a pothole → Should approve with high confidence
2. Upload a photo of trash → Should approve
3. Upload a photo of a broken streetlight → Should approve

### Bad Test Images:
1. Upload a screenshot → Should reject with "not a civic issue"
2. Upload a selfie → Should reject
3. Upload a photo of food → Should reject

## 📊 Console Logs

Watch the console to see the validation process:

```bash
🔍 Starting image analysis...
✅ Image description: a road with potholes and cracks
🎯 Analyzing description against categories...
  ✓ pothole: matched "pothole/hole" (weight: 1.0)
📊 Category scores: { pothole: 1.0, water: 0, trash: 0, ... }
🏆 Best match: pothole (score: 1.0)
✅ Final result: pothole with 85% confidence

🤖 AI Analysis Result: { isAppropriate: true, suggestedCategory: 'pothole', ... }
✅ Auto-filling category: pothole
```

Or for rejected images:

```bash
🔍 Starting image analysis...
✅ Image description: web site, website, internet site
❌ Non-civic issue detected: web site, website, internet site

🤖 AI Analysis Result: { isAppropriate: false, reason: '...' }
❌ Image rejected: Unable to identify a civic infrastructure issue...
```

## 💡 User Experience

### What Users See:

1. **Upload Image** → Loading spinner appears
2. **AI Analyzes** → 2-7 seconds processing
3. **Result:**
   - ✅ **Approved:** Category auto-selected, green checkmark
   - ❌ **Rejected:** Red error box, image removed, upload again

### Error Message Example:
```
⚠️ Image Rejected

Unable to identify a civic infrastructure issue in this image.
Please upload a clear photo showing road damage, potholes, water
leaks, trash, electrical problems, or other municipal
infrastructure issues.
```

## 🆓 Still 100% Free!

All image validation happens using Hugging Face's free models - no extra cost!

## 📝 Summary

- ✅ Only civic infrastructure issue photos are accepted
- ✅ AI automatically validates and categorizes
- ✅ Clear error messages guide users
- ✅ Prevents spam and irrelevant reports
- ✅ Completely free to use

This ensures your civic reporting app only contains **legitimate infrastructure issues**, keeping the platform focused and useful! 🎯
