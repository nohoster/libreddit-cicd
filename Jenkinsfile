podTemplate(yaml: '''
              apiVersion: v1
              kind: Pod
              spec:
                volumes:
                - name: containerdir
                  emptyDir: {}
                containers:
                - name: buildah
                  image: quay.io/buildah/stable:latest
                  command:
                  - sleep
                  args:
                  - 99d
                  volumeMounts:
                  - name: containerdir
                    mountPath: /var/lib/containers
''') {
  node(POD_LABEL) {
    container('buildah') {
      git branch: 'master', url: 'https://github.com/libreddit/libreddit.git'
      sh 'buildah --storage-driver=vfs manifest create test'
      sh 'buildah build --arch amd64 --tag "repocr.azurecr.io/libreddit:latest" --manifest "test" .'
    }
  }
}
