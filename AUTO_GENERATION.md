# AI Auto-Generation Feature

Your app now automatically generates **title and description** for complaints based on the uploaded image!

## ✨ How It Works

When you upload an image:

1. **AI analyzes the image** → Detects category (pothole, water, trash, etc.)
2. **Generates title** → Creates a descriptive title based on the issue type
3. **Generates description** → Writes a detailed description explaining the problem
4. **Auto-fills form** → Title, description, and category are filled automatically

You can edit any of these fields if you want to customize them!

## 🎯 Example Flow

### Upload Pothole Image

**What happens:**
```
1. Upload image of road with potholes
   ↓
2. AI detects: "Pothole" (85% confidence)
   ↓
3. Auto-generates:
   - Title: "Multiple Potholes Detected"
   - Description: "Several potholes have formed on this road section.
     The deteriorating pavement condition could lead to vehicle
     damage and requires prompt repair work."
   - Category: Pothole
   ↓
4. All fields filled automatically!
   ↓
5. You can edit them if you want, or just submit
```

**What you see:**
```
Title: Multiple Potholes Detected
✨ AI-generated (you can edit this)

Category: Pothole

Description: Several potholes have formed on this road section...
✨ AI-generated (you can edit this)
```

## 📝 Generated Templates by Category

### 🕳️ Pothole
**Titles:**
- "Potholes on Road"
- "Road Surface Damage"
- "Multiple Potholes Detected"
- "Damaged Road Pavement"
- "Road Repair Needed"

**Description Example:**
> Multiple potholes detected on the road surface causing potential hazard to vehicles. The road pavement shows significant damage and requires immediate repair.

### 💧 Water
**Titles:**
- "Water Leak Detected"
- "Flooding on Street"
- "Drainage Issue"
- "Water Accumulation Problem"
- "Broken Water Pipe"

**Description Example:**
> Water leak or flooding observed on the street. This could be from a broken pipe or drainage issue that requires immediate investigation and repair.

### 🗑️ Trash
**Titles:**
- "Trash Accumulation"
- "Illegal Dumping Detected"
- "Overflowing Garbage"
- "Waste Disposal Issue"
- "Littering Problem"

**Description Example:**
> Excessive trash and garbage accumulation observed at this location. Regular cleanup and proper waste management is needed to maintain community hygiene.

### ⚡ Electrical
**Titles:**
- "Streetlight Not Working"
- "Electrical Hazard Detected"
- "Broken Street Light"
- "Power Line Issue"
- "Lighting Problem"

**Description Example:**
> Streetlight malfunction or electrical issue detected. This affects public safety and visibility, requiring urgent attention from the electrical maintenance team.

### 🛣️ Road
**Titles:**
- "Road Damage Detected"
- "Damaged Road Sign"
- "Broken Barrier"
- "Road Infrastructure Issue"
- "Street Maintenance Required"

**Description Example:**
> Road infrastructure damage observed including possible issues with signs, barriers, or road surface. This requires inspection and repair by the road maintenance department.

### 📋 Other
**Titles:**
- "Civic Infrastructure Issue"
- "Public Property Damage"
- "Municipal Maintenance Required"
- "Community Infrastructure Problem"
- "Public Facility Issue"

**Description Example:**
> Infrastructure or public property issue detected that requires municipal attention. Please inspect this location to determine the specific problem and necessary repairs.

## 🎨 Smart Template Selection

The AI doesn't just randomly pick a template - it analyzes the image description:

| Image Contains | Selected Title |
|---------------|----------------|
| "multiple" or "several" | "Multiple Potholes Detected" |
| "damage" or "broken" | "Road Surface Damage" |
| "manhole" or "cover" | "Damaged Road Pavement" |
| Default | First template in list |

## ✏️ Editing Auto-Generated Content

### You Can Always Edit!

All auto-generated fields are fully editable:

1. **Title** - Click and type to change
2. **Description** - Edit or completely rewrite
3. **Category** - Change from dropdown if AI got it wrong

The "✨ AI-generated" label disappears once you edit the field.

### When to Edit:

**Keep AI-generated:**
- ✅ If it accurately describes your issue
- ✅ Saves you time typing
- ✅ Sounds professional

**Edit it:**
- ✏️ Add specific details (address, severity)
- ✏️ Include additional context
- ✏️ Make it more personal
- ✏️ Fix any inaccuracies

## 🔧 Technical Details

### For Developers

The title and description generation happens in:
[app/api/analyze-image/route.ts](app/api/analyze-image/route.ts)

**Function:**
```typescript
function generateTitleAndDescription(
  category: Category,
  imageDescription: string,
  confidence: number
): { title: string; description: string }
```

**Returns:**
```typescript
{
  suggestedTitle: string,
  suggestedDescription: string
}
```

### Customizing Templates

To add or modify templates, edit the `templates` object in `generateTitleAndDescription()`:

```typescript
const templates = {
  pothole: {
    titles: [
      'Potholes on Road',
      'Your Custom Title Here',  // Add more!
      // ...
    ],
    descriptions: [
      'Your custom description...',
      // ...
    ]
  }
}
```

### Template Selection Logic

```typescript
// Keywords in image description affect which template is chosen
if (imageDescription.includes('multiple')) {
  titleIndex = 2  // "Multiple Potholes Detected"
} else if (imageDescription.includes('damage')) {
  titleIndex = 1  // "Road Surface Damage"
}
```

## 📊 Benefits

### For Users:
- ⚡ **Faster reporting** - No typing required
- ✍️ **Better descriptions** - Professional, detailed text
- 🎯 **Consistency** - All reports follow similar format
- 🔄 **Still flexible** - Can edit anything

### For Admins:
- 📝 **Consistent data** - Reports use standardized language
- 🔍 **Better searchability** - Similar issues use similar keywords
- 📈 **Easier categorization** - Clear, descriptive titles
- 🤖 **Automated** - Less manual correction needed

## 🆓 Still 100% Free!

The title and description generation:
- ✅ Uses the same AI model (no extra cost)
- ✅ No additional API calls
- ✅ Included in the image analysis
- ✅ Completely free!

## 🎮 Try It!

### Test 1: Upload Pothole Image
1. Click "Report Issue"
2. Upload pothole photo
3. Watch fields auto-fill!
4. Edit if you want
5. Submit

### Test 2: Upload Water Leak
1. Upload flooding/water leak photo
2. See water-related title and description
3. Category auto-selected as "Water"

### Test 3: Edit Auto-Generated Content
1. Upload any civic issue image
2. See AI-generated title
3. Click in the title field and edit it
4. Notice the ✨ label disappears

## 🌟 Future Enhancements

Possible improvements:
- 📍 Include location in description
- 📅 Add timestamp information
- 🌡️ Include severity level
- 🔤 Multi-language support
- 🎨 Different writing styles (formal/casual)

## ❓ FAQ

**Q: Will AI always generate perfect descriptions?**
A: No - that's why you can edit them! Use the AI-generated text as a starting point.

**Q: Can I turn off auto-generation?**
A: Currently, it's always on. But since fields are editable, you can clear and write your own.

**Q: What if AI gets the category wrong?**
A: Just change it from the dropdown! The title/description won't automatically update though.

**Q: Does it work in other languages?**
A: Currently English only. The AI model analyzes in English.

**Q: Can I suggest better templates?**
A: Yes! Edit the templates in the code or suggest improvements.

---

**This feature saves time while maintaining report quality!** 🚀
