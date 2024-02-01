func getChunkIpsVolumeAllocated(client *zbs.Client, ctx context.Context, volumeId string) ([]string, error) {
	chunks, err := client.Meta.Chunk.ListChunk(ctx)
	if err != nil {
		return nil, err
	}
	rsp, err := client.Meta.ShowVolume(ctx, &metaproto.VolumePath{
		VolumeId: []byte(volumeId),
	})
	if err != nil {
		return nil, err
	}

	chunks, err := client.Meta.Chunk.ListChunk(ctx)
	if err != nil {
		return nil, err
	}

	cidChunkIpMap := make(map[uint32]string)
	for _, chunk := range chunks.GetChunks() {
		rpcIp := cmdutil.Int2ip(chunk.GetRpcIp())
		cidChunkIpMap[*chunk.Id] = rpcIp
	}

	set := make(map[uint32]bool)
	for _, pextent := range rsp.Pextents {
		replicas := cmdutil.LocationToUint32Slice(pextent.GetLocation())
		if len(replicas) == 0 {
			continue
		}
		for _, r := range replicas {
			set[r] = true
		}
	}

	chunksList := make([]string, 0, len(set))
	for key := range set {
		chunksList = append(chunksList, cidChunkIpMap[key])
	}

	return chunksList, nil
}


 wget https://repo.oepkgs.net/openeuler/rpm/openEuler-20.03-LTS-SP1/extras/aarch64/Packages/b/babeltrace2-2.0.4-4.aarch64.rpm
 wget https://repo.oepkgs.net/openeuler/rpm/openEuler-20.03-LTS-SP1/extras/aarch64/Packages/b/babeltrace2-debuginfo-2.0.4-4.aarch64.rpm
 wget https://repo.oepkgs.net/openeuler/rpm/openEuler-20.03-LTS-SP1/extras/aarch64/Packages/l/libbabeltrace2-2.0.4-4.aarch64.rpm
 wget https://repo.oepkgs.net/openeuler/rpm/openEuler-20.03-LTS-SP1/extras/aarch64/Packages/l/libbabeltrace2-devel-2.0.4-4.aarch64.rpm
 
 
  yum install http://192.168.17.20/repo/pub/smartxos/el7/smtx/x86_64/libzbs-6.0.0-rc9.0.release.git.g0df8200c2.el7.SMTX.HCI.x86_64.rpm http://192.168.17.20/repo/pub/smartxos/el7/smtx/x86_64/libzbs-devel-6.0.0-rc9.0.release.git.g0df8200c2.el7.SMTX.HCI.x86_64.rpm
  http://192.168.17.20/repo/pub/smartxos/el7/smartx-iso-common/x86_64/libfdisk-2.27-0.4.el7.x86_64.rpm
  http://192.168.17.20/repo/pub/smartxos/el7/smartx-iso-common/x86_64/userspace-rcu-0.8.6-2.el7.centos.x86_64.rpm