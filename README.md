# AGRISENSE.AI

**Live App:** https://aqua-agro-wise.lovable.app/

## Overview

**AgriSense.AI** is a mobile-first AI agriculture platform designed to help South African farmers diagnose crop, livestock, soil, and irrigation problems from a single photo.

The platform takes a **water-first approach**, providing practical recommendations based on the diagnosis while demonstrating how multimodal AI can support agricultural decision-making.

## Key Features

1. **AI Photo Diagnosis**
   - Upload a crop, livestock, soil, or irrigation image
   - AI analyses the image and identifies possible issues
   - Provides practical, prioritised recommendations

2. **AI Agriculture Advisor**
   - Chat-based support for agriculture and water-management questions
   - Provides South Africa-focused guidance

3. **One-Tap Sample Diagnosis**
   - Six built-in sample scenarios
   - Demonstrates the complete diagnosis workflow without requiring an upload

4. **Water-First Recommendations**
   - Irrigation efficiency
   - Leak detection
   - Mulching
   - Rainwater harvesting
   - Drought-conscious farming practices

5. **Mobile-First Design**
   - Camera-friendly image uploads
   - Large touch targets
   - Designed with rural and low-data accessibility in mind

## How It Works

1. Farmer uploads an image or selects a sample.
2. The image is converted to Base64.
3. The frontend calls the `agri-ai` Supabase Edge Function.
4. The Edge Function validates the request and sends it to the AI gateway.
5. Gemini analyses the image and returns recommendations.
6. Results are displayed as a structured action plan.

## Technology Stack

- **Frontend:** React 19, TanStack Start, TypeScript
- **Styling:** Tailwind CSS v4
- **Build Tool:** Vite
- **Backend:** Supabase Edge Functions / Lovable Cloud
- **AI:** Google Gemini 2.5 Flash via Lovable AI Gateway
- **UI:** shadcn-style components
- **Markdown:** React Markdown
- **Notifications:** Sonner

