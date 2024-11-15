##### Module 3: Feed Configuration

**Overview**
This module covers the setup and optimisation of FreshRSS feeds, focusing on creating a clean, distraction-free reading experience. We'll explore the realities of RSS feed configuration, including the limitations of content extraction and potential alternative solutions.

**Learning Objectives**
- Understanding RSS feed structure and management
- Learning about content extraction challenges
- Implementing CSS selectors for content parsing
- Setting up social media integration
- Optimising the reading experience

**Prerequisites**
- Completed Module 0 (Project Initialisation)
- Completed Module 1 (Environment Setup)
- Completed Module 2 (FreshRSS Deployment)
- Working FreshRSS installation accessible via browser

**Adding News Sources**
Begin by accessing your FreshRSS installation through the browser and navigating to the subscription management page:

```bash
# RSS feed URLs for major news sources
Reuters: https://www.reuters.com/rssfeed
AP News: https://www.ap.org/rss
EuroNews: https://www.euronews.com/rss
Al Jazeera: https://www.aljazeera.com/xml/rss/all.xml
TechCrunch: https://techcrunch.com/feed
Forbes: https://www.forbes.com/real-time/feed2
```

For each source:
1. Select 'New Subscription'
2. Paste the RSS feed URL
3. Choose appropriate category
4. Set refresh interval (typically 1 hour for news)

**Content Extraction Realities**
It's important to note that full article extraction from major news sources can be challenging in FreshRSS. Several factors contribute to this:
- Publishers often provide truncated RSS feeds
- Website structures frequently change
- Paywalls and authentication requirements
- Dynamic content loading

Alternative solutions to consider:
- Tiny Tiny RSS (TTRSS) with full-text plugins
- FiveFilters Full-Text RSS service
- Self-hosted feed parsing services

**CSS Selector Implementation**
To extract content where possible, configure CSS selectors:
1. Navigate to FreshRSS subscription settings
2. Access the feed's configuration
3. Add relevant CSS selectors:
```css
/* Example selectors - adjust based on source */
.article-content
.main-story-content
.story-body__inner
.article__body
```

Note: CSS selectors need regular maintenance as news sites update their layouts.

**Social Media Integration**
FreshRSS supports various social media platforms through specific RSS bridges:

```bash
# Install RSS Bridge extension
cd freshrss/extensions
git clone https://github.com/RSS-Bridge/rss-bridge.git
```

Configure bridges for:
- Twitter (now X) using Nitter RSS feeds
- YouTube channel RSS feeds (format: https://www.youtube.com/feeds/videos.xml?channel_id=CHANNEL_ID)
- Reddit subreddit feeds (append .rss to subreddit URL)

**Reading Experience Optimisation**
Configure reading settings:
1. Enable dark mode for reduced eye strain
2. Set comfortable font size and line spacing
3. Configure swipe gestures for mobile reading
4. Enable keyboard shortcuts for efficient navigation
5. Set up reading view preferences:
   - Article width
   - Image handling
   - Link behaviour

**Success Criteria**
- News sources successfully added
- Content extraction working where possible
- Clean, distraction-free reading experience
- Social media feeds integrated
- Understanding of system limitations

**Common Issues and Solutions**
- Limited content extraction: Consider alternative RSS readers
- Feed parsing errors: Verify URL format
- Social media integration failures: Verify bridge configuration
- Performance issues: Adjust refresh intervals

**Next Steps**
After completing this module:
1. Test all configured feeds
2. Document which sources provide full content
3. Research alternative solutions for problematic feeds
4. Check mobile reading experience
5. Prepare for Module 4: Security Implementation

**Additional Resources**
- TTRSS documentation
- FiveFilters services
- CSS selector guides
- Feed validation tools

**Notes**
- Regular maintenance of CSS selectors required
- Consider multiple RSS solutions for different sources
- Document working configurations
- Keep track of which methods work best for specific sources
