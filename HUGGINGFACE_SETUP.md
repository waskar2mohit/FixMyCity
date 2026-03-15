# Complete Setup Guide - 100% Free!

This project uses **completely FREE services** for everything:
- ✅ **Hugging Face** for AI image analysis (no billing required!)
- ✅ **Supabase** for database and image storage (1GB free!)
- ✅ No credit card needed for either service!

## Part 1: Hugging Face API Setup (AI Image Analysis)

### Getting Your Free Hugging Face API Key

1. **Create a Hugging Face Account** (if you don't have one)
   - Go to [https://huggingface.co/join](https://huggingface.co/join)
   - Sign up with your email or GitHub account
   - It's completely FREE - no credit card required!

2. **Generate an API Token**
   - Visit [https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)
   - Click on **"New token"**
   - Give it a name (e.g., "FixMyCity")
   - Select **"Read"** permission (this is enough for inference)
   - Click **"Generate a token"**
   - Copy the token that appears

3. **Add the Token to Your .env File**
   - Open the `.env` file in your project root
   - Add your token:
   ```
   HUGGINGFACE_API_KEY=hf_xxxxxxxxxxxxxxxxxxxxxxxxx
   ```

## Part 2: Supabase Storage Setup (Image Storage)

You also need to set up Supabase storage for complaint images. See the detailed guide:
👉 **[SUPABASE_STORAGE_SETUP.md](SUPABASE_STORAGE_SETUP.md)**

Quick summary:
1. Go to your Supabase dashboard
2. Create a bucket named `sem6_fixmycity` (mark as public)
3. Set up storage policies for uploads and public access

## Part 3: Start Your App

After completing both setups above:

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000)

## What Changed?

### Before (Google Gemini)
- ❌ Required billing account for image analysis
- ❌ Used `GOOGLE_GENERATIVE_AI_API_KEY`
- ❌ Limited free tier
- ❌ Pay per request

### After (Hugging Face + Supabase)
- ✅ **100% FREE** - No billing required
- ✅ Uses `HUGGINGFACE_API_KEY`
- ✅ Access to thousands of open-source models
- ✅ Uses `nlpconnect/vit-gpt2-image-captioning` (ViT-GPT2) for accurate image understanding
- ✅ Fallback to `google/vit-base-patch16-224` for image classification if needed
- ✅ Supabase free tier: 1GB storage for images

## How It Works Now

The image analysis uses **ViT-GPT2** (Vision Transformer + GPT-2), a powerful open-source vision model available in Hugging Face's free tier:

1. **Image Captioning**: ViT-GPT2 generates a detailed description of the uploaded image (with fallback to image classification)
2. **Content Filtering**: Checks for inappropriate content (violence, explicit material, etc.)
3. **Smart Categorization**: Uses advanced keyword matching with weighted scoring to identify:
   - 🕳️ **Potholes**: Holes, cracks, pavement damage
   - 💧 **Water Issues**: Leaks, flooding, drainage problems
   - 🗑️ **Trash**: Garbage, litter, illegal dumping
   - ⚡ **Electrical**: Broken streetlights, exposed wires
   - 🛣️ **Road**: Damaged signs, barriers, general road issues
   - 📋 **Other**: Any other civic problems

4. **Confidence Scoring**: Returns high confidence scores (50-85%) based on match quality
   - 85%+ for exact matches (e.g., "pothole" detected)
   - 65-75% for strong indicators
   - 50-65% for moderate matches
   - 35-50% for weak matches

## Free Tier Limits

Hugging Face's free tier includes:
- Rate limiting (reasonable for most applications)
- Cold starts (first request may be slower)
- No SLA guarantee

For production use with higher traffic, you may want to upgrade to Hugging Face Pro.

## Troubleshooting

**Issue**: "API key not configured" error
- Make sure you've added `HUGGINGFACE_API_KEY` to your `.env` file
- Ensure there are no spaces around the `=` sign
- Restart your dev server after adding the key

**Issue**: Slow first request
- This is normal - Hugging Face loads the model on first use (cold start)
- Subsequent requests will be faster

**Issue**: Rate limit errors
- You may be making too many requests
- Consider implementing request caching or throttling
- Upgrade to Hugging Face Pro if needed

## Why BLIP is Better

We switched from VQA (Visual Question Answering) to BLIP Image Captioning because:

| Feature | VQA Model | BLIP Model |
|---------|-----------|------------|
| Accuracy | 30-40% confidence | **50-85% confidence** ✅ |
| Understanding | Simple yes/no answers | Detailed image descriptions ✅ |
| Category Detection | Limited keyword matching | Advanced weighted scoring ✅ |
| Model Size | Small (faster, less accurate) | Large (better accuracy) ✅ |

**Result**: Much more accurate categorization of civic issues!

## Alternative Models (Advanced)

If you want to experiment with different models, you can change the model in [app/api/analyze-image/route.ts](app/api/analyze-image/route.ts:32):

```typescript
const captionResponse = await hf.imageToText({
  model: 'your-preferred-model-here',
  data: new Blob([imageBlob], { type: imageFile.type })
})
```

Some alternatives for image captioning:
- `Salesforce/blip-image-captioning-base` - Faster, slightly less accurate (current: large)
- `nlpconnect/vit-gpt2-image-captioning` - Good alternative
- `microsoft/git-large-coco` - Microsoft's image captioning model

Browse more models at [https://huggingface.co/models?pipeline_tag=image-to-text](https://huggingface.co/models?pipeline_tag=image-to-text)
