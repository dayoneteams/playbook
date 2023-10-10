# Tag new version

Follow Semantic Versioning.
Assuming you're using `master` branch as the main branch.
```bash
git checkout master
npm version <patch|minor|major>
git push origin master --tags
```
