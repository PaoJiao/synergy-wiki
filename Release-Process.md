## Checklist

**Note:** Takes approx 1-2 hours (depending on testing).

1. Create release branch (e.g. 1.2.3-stable)
2. Copy [milestone](https://github.com/synergy/synergy-enterprise/milestones) to ChangeLog
3. Ensure version in CMakeLists.txt is correct
4. Push and wait for buildbot to build (be patient)
5. Wait for packages to be uploaded to webserver
6. Test new bug fixes and features on all operating systems
7. Tag the release branch (same name as branch)
8. Merge release branch into master
9. Delete the release branch
10. Close the release milestone
11. Upload to public website and test links
12. Set current version in website settings
13. Install previous version and test update check feature
14. **If master:** set next version in CMakeLists.txt

## Announce

This should be done for major minor releases, but not for maintenance releases.

1. Post on [Synergy blog](http://synergyopensource.wordpress.com/) (same as email)
2. Email subscription list (include blog link)
3. Message mailing list (include ChangeLog)
4. Post on the [Synergy Forums](http://synergy-project.org/forum/)
5. Post on [Facebook page](https://www.facebook.com/SynergyOpenSource) (include blog link)
6. Post on [Twitter](https://twitter.com/SynergyDev) (include blog link)
7. Post on [Google+ page](https://plus.google.com/b/109104035534174281072/+Synergy/posts) (include blog link)
8. Share on [LinkedIn](https://www.linkedin.com/home)
9. Share on [StumbleUpon](https://www.stumbleupon.com/)
10. Share on [reddit](https://www.reddit.com/)
11. Update Wikipedia versions ([en](http://en.wikipedia.org/wiki/Synergy_(software)), [de](http://de.wikipedia.org/wiki/Synergy_(Software)), [fr](http://fr.wikipedia.org/wiki/Synergy_(logiciel)) [es](http://es.wikipedia.org/wiki/Synergy), [no](http://no.wikipedia.org/wiki/Synergy_(programvare)), [ru](http://ru.wikipedia.org/wiki/Synergy_(%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0)))

## Chinese

1. Post on [Renren page](http://page.renren.com/601718008)
2. Post on Weibo ([左耳朵耗子](http://weibo.com/haoel) [IT程序猿](http://weibo.com/kuqin))